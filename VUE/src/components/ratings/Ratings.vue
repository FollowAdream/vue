<template>
    <div class="ratings">
        <div class="ratings-top" v-if="seller">
          <div class="score">
            <label>{{seller.score}}</label>
            <p>综合评分</p>
            <span>高于周边商家{{seller.rankRate}}%</span>
          </div>
          <div class="service">
            <div class="star">
              <p>服务态度</p>
              <mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>
              <span>{{seller.serviceScore}}</span>
            </div>
            <div class="star">
              <p>服务态度</p>
              <mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>
              <span>{{seller.foodScore}}</span>
            </div>
            <p>送达时间<span class="time">{{seller.deliveryTime}}分钟</span></p>
          </div>
        </div>
      <div class="bg"></div>
      <div class="evaluate" v-if="ratings">
        <div class="evaluate-top">
          <div class="top">
            <p class="whole">全部 <span>{{ratings.length}}</span></p>
            <p class="Satisfied">满意<span>24</span></p>
            <p class="Dissatisfied">不满意<span>0</span></p>
          </div>
          <div class="btn">
            <i class="icon-check_circle"></i>
            <span>评价的内容</span>
          </div>
        </div>
        <ul class="evaluate-btn">
          <li v-for="rating in ratings">
            <img :src="rating.avatar" alt="">
            <div class="rating-main">
              <h5>{{rating.username}}</h5>
              <div class="star">
                <mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>
                <span>{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p>{{rating.text}}</p>
            </div>
          </li>
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
        ratings:null
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
      this.$http.get("/api/ratings").then((res) => {
        this.ratings=res.data.data;
        console.log(this.ratings);
      },(err) => {
        console.log(err);
      });
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
  .ratings{
    .ratings-top{
      padding: 18px 0;
      display: flex;
      .score{
        flex: 2;
        text-align: center;
        label{
          font-size: 24px;
          color: rgb(255,153,0);
          line-height: 28px;
        }
        p{
          margin-top: 6px;
          font-size: 14px;
          color: rgb(7,17,27);
          line-height: 14px;
          margin-bottom: 8px;
        }
        span{
          font-size: 10px;
          line-height: 10px;
          color: rgb(147,153,159);
        }
      }
      .service{
        flex: 3;
        padding: 0 24px;
        border-left: 1px solid #e6e7e8;
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
        .star{
          display: flex;
          margin-bottom: 8px;
          p{
            margin-right: 12px;
          }
          span{
            margin-left: 12px;
            color: rgb(255,153,0);
          }
        }
        .time{
          margin-left: 12px;
          color: rgb(147,153,159);
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
    .evaluate{
      .evaluate-top{
        padding: 18px;
        .top{
          padding-bottom: 18px;
          display: flex;
          border-bottom: 1px solid #e6e7e8;
          .whole{
            padding: 0 12px;
            background: #00a0dc;
            line-height: 33px;
            border-radius: 3px;
            margin-right: 8px;
          }
          .Satisfied{
            padding: 0 12px;
            background: #ccecf8;
            line-height: 33px;
            border-radius: 3px;
            margin-right: 8px;
          }
          .Dissatisfied{
            padding: 0 12px;
            background: #e9ebec;
            line-height: 33px;
            border-radius: 3px;
          }
        }
        .btn{
          padding-top: 18px;
          .icon-check_circle{
            color: #b7bbbf;
            font-size: 20px;
            vertical-align:middle;
          }
          span{
            font-size: 14px;
            line-height: 14px;
            color: #93999f;
          }
        }
      }
      .evaluate-btn{
        border-top: 2px solid #e6e7e8;
        padding: 0 18px;
        li{
          padding: 18px 0;
          display: flex;
          border-bottom: 1px solid #e6e7e8;
          img{
            width: 28px;
            height: 28px;
            border-radius: 50%;
            margin-right: 12px;
          }
          .rating-main{
            h5{
              font-size: 10px;
              color: rgb(7,17,27);
              line-height: 12px;
              margin-bottom: 4px;
            }
            .star{
              display: flex;
              span{
                margin-left: 6px;
                font-size: 10px;
                font-weight: 200;
                color: rgb(174,153,159);
                line-height: 20px;
              }
            }
          }
          p{
            font-size: 12px;
            color: rgb(7,17,27);
            line-height: 18px;
          }
        }
      }
    }
  }
</style>
