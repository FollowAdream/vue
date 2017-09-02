<template>
    <div class="seller" v-if="seller">
        <div class="seller-top">
          <div class="seller-title">
           <div class="title-l">
             <h3 class="name">{{seller.name}}</h3>
             <div class="start-wrapper">
               <mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>
               <span class="ratingCount">({{seller.ratingCount}})</span>
               <span class="sellCount">月售{{seller.sellCount}}单</span>
             </div>
           </div>
            <div class="seller-r">
              <div class="collect">
                <i class="icon-favorite"></i>
                <p class="text">已收藏</p>
              </div>
            </div>
          </div>
          <ul class="seller-btn">
            <li class="btn-li">
              <p>起送价</p>
              <span>{{seller.minPrice}}</span>元
            </li>
            <li class="btn-li">
              <p>商家配送</p>
              <span>{{seller.deliveryPrice}}</span>元
            </li>
            <li class="btn-li">
              <p>平均配送时间</p>
              <span>{{seller.deliveryTime}}</span>元
            </li>
          </ul>
        </div>
      <div class="bg"></div>
      <div class="notice">
        <h3>公告与活动</h3>
        <div class="text">
          <p>{{seller.bulletin}}</p>
          <div class="supports" v-for="sell in seller.supports">
            <span class="icon" :class="picMap[sell.type]"></span>
            <span class="text">{{sell.description}}</span>
          </div>
        </div>
      </div>
      <div class="bg"></div>
      <div class="scene">
        <h3>商家实景</h3>
        <ul>
          <li>
            <img :src="seller.pics[0]" alt="">
          </li>
          <li>
            <img :src="seller.pics[1]" alt="">
          </li>
          <li>
            <img :src="seller.pics[2]" alt="">
          </li>
        </ul>
      </div>
      <div class="bg"></div>
      <div class="Information ">
        <h3>商家信息</h3>
        <ul class="main">
          <li v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
      <shopcart v-if="seller" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    </div>
</template>

<script>
  import Alltar from '../allstar/allstar.vue';
  import Shopcart from '../shopcart/Shopcart.vue';
  export default{
    data(){
      return {
        seller:null,
        picMap:[],
      };
    },
    created(){
      //获取seller接口数据
      this.$http.get("/api/seller").then((res) => {
        this.seller=res.data.data;
        console.log(this.seller);
      },(err) => {
        console.log(err);
      });
      this.picMap = ["decrease", "discount", "guarantee", "invoice", "special"];
    },
    methods:{
      showInfo:function (data) {
        this.username=data;
      }
    },
    components: {
      mystar:Alltar,
      shopcart: Shopcart
    }

  }
</script>

<style lang="less" rel="stylesheet/less">
  .seller{
    margin-bottom: 50px;
    .seller-top{
      height: auto;
      padding: 18px;
      .seller-title{
        display: flex;
        border-bottom: 1px solid rgba(7,17,27,0.1);
        .title-l{
          display: inline-block;
          flex: 3;
          .name{
            font-size: 14px;
            color: rgb(7,17,27);
            line-height: 14px;
            margin-bottom: 8px;
          }
          .start-wrapper{
            display: flex;
            font-size: 10px;
            line-height: 18px;
            color: rgb(77,85,93);
            .ratingCount{
              display: inline-block;
              margin-left: 8px;
              margin-right: 12px;
            }
          }
        }
        .seller-r{
          display: inline-block;
          flex: 1;
          .collect{
            display: inline-block;
            float: right;
            text-align: center;
            .icon-favorite{
              font-size: 24px;
              color: rgb(240,24,24);
              line-height: 24px;
            }
            .text{
              font-size: 10px;
              color: rgb(77,85,93);
            }
          }
        }
      }
      .seller-btn{
        display: flex;
        padding-top: 18px;
        text-align: center;
        .btn-li{
          flex: 1;
          border-right: 1px solid #e6e7e8;
          font-size: 10px;
          &:last-child{
            border-right: none;
          }
          p{
            font-size: 10px;
            color: rgb(147,153,159);
            line-height: 10px;
            margin-bottom: 4px;
          }
          span{
            font-size: 24px;
            font-weight: 200;
            color: rgb(7,17,27);
            line-height: 24px;
          }
        }
      }
    }
    .bg{
      width: 100%;
      height: 16px;
      background: #f3f5f7;
      border-bottom: 1px solid rgba(7,17,27,0.1);
      border-top: 1px solid rgba(7,17,27,0.1);
    }
    .notice{
      padding: 18px 18px 0 18px;
      h3{
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
        margin-bottom: 8px;
      }
      .text{
        padding: 0 12px;
        p{
          font-size: 12px;
          color: rgb(240,20,20);
          line-height: 24px;
          font-weight: 200;
          margin-bottom: 16px;
        }
        .supports{
          padding: 16px 12px;
          border-top: 1px solid #e6e7e8;
          .icon{
            display: inline-block;
            width: 16px;
            height: 16px;
            background-size: 16px 16px;
            vertical-align:middle;
            &.decrease{
              background-image: url(decrease_2@2x.png);
            }
            &.discount {
              background-image: url(discount_2@2x.png);
            }
            &.guarantee {
              background-image: url(guarantee_2@2x.png);
            }
            &.invoice {
              background-image: url(invoice_2@2x.png);
            }
            &.special {
              background-image: url(special_2@2x.png);
            }
          };
          .text{
            font-size: 12px;
            font-weight: 200;
            line-height: 12px;
          }
        }
      }
    }
    .scene{
      padding: 18px;
      h3{
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
        margin-bottom: 12px;
      }
      ul{
        display: flex;
        li{
          width: 120px;
          height: 90px;
          margin-right: 6px;
          &:last-child{
            margin-right: 0;
          }
          img{
            width: 100%;
            height: 100%;
          }
        }
      }
    }
    .Information{
      padding: 18px;
      h3{
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
        margin-bottom: 12px;
      }
      ul{
        display: inline-block;
        li{

          padding: 16px 12px;
          border-top: 1px solid #e6e7e8;
          font-size: 12px;
          color: rgb(7,17,27);
          line-height: 16px;
        }
      }
    }
  }
</style>
