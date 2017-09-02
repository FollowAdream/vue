<template>
    <div class="header" v-if="seller">
        <div class="content-wrapper" >
            <div class="avatar">
              <img :src="seller.avatar" alt="" width="64">
            </div>
            <div class="content">
                <div class="title">
                  <span class="brand"></span>
                  <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
                <div class="supports">
                  <span class="icon" :class="picMap[seller.supports[0].type]"></span>
                  <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div class="support-count" @click="showDetail">
              <span class="count">{{seller.supports.length}}个</span>
              <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
      <!--模糊背景-->
      <div class="background">
        <img :src="seller.avatar" width="100%" height="100%" alt="">
      </div>

      <transition name="slide">
        <div class="detail" v-show="detailShow">
          <div class="detail-wrapper clearfix" >
            <div class="detail-main">
              <h3 class="name">{{seller.name}}</h3>
              <div class="start-wrapper">
                <mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>
              </div>
              <div class="title">
                <div class="line1"></div>
                <h4 class="name">优惠信息</h4>
                <div class="line2"></div>
              </div>
              <div class="supports" v-for="sell in seller.supports">
                <span class="icon" :class="picMap[sell.type]"></span>
                <span class="text">{{sell.description}}</span>
              </div>
              <div class="notice">
                <div class="line1"></div>
                <h4 class="name">商家公告</h4>
                <div class="line2"></div>
              </div>
              <p class="notice-text">{{seller.bulletin}}</p>
              <div class="detail-close" @click="hideDetail">
                <i class="icon-close"></i>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
</template>

<script>
  import Alltar from '../allstar/allstar.vue';
    export default{
        data(){
            return {
                seller:null,
                picMap:[],
                detailShow:false
            };
        },
        methods:{
          showDetail(){
              this.detailShow=true;
          },
          hideDetail(){
              this.detailShow=false;
          },
          showInfo:function (data) {
            this.username=data;
          }
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

      components: {
        mystar:Alltar,

      }

    }
</script>


<style lang="less" rel="stylesheet/less">
  .header{
    width: 100%;
    height:134px;
    background: rgba(7, 17, 27, 0.5);
    color: #fff;
    overflow: hidden;
    /*display: block;*/
    position: relative;
    .content-wrapper{
      display: flex;
      .avatar{
        display: inline-block;
        margin: 24px 16px 18px 24px;
        img{
          border-radius: 2px;
        }
      };
      .content{
        flex: 1;
        .title{
          margin-top: 26px;
          margin-bottom: 8px;
          .brand{
            vertical-align: top;
            display: inline-block;
            width: 30px;
            height: 18px;
            background: url("brand@2x.png");
            background-size: 30px 18px;
          };
          .name{
            font-weight: 300;
            line-height: 18px;
            display: inline-block;
            margin-left: 6px;
          }
        }
        .description{
          font-size: 12px;
          font-weight: 200;
          line-height: 12px;
          margin-bottom: 10px;
        };
        .supports{
          .icon{
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: 12px 12px;
            vertical-align:middle;
            &.decrease{
              background-image: url(decrease_1@2x.png);
            }
            &.discount {
              background-image: url(discount_1@2x.png);
            }
            &.guarantee {
              background-image: url(guarantee_1@2x.png);
            }
            &.invoice {
              background-image: url(invoice_1@2x.png);
            }
            &.special {
              background-image: url(special_1@2x.png);
            }
          };
          .text{
            font-size: 10px;
            font-weight: 200;
            line-height: 12px;
          }
        }
      }
      .support-count{
        width:48px;
        height: 24px;
        background-color:rgba(0,0,0,0.2);
        border-radius: 10px;
        position: absolute;
        right: 14px;
        bottom:40px;
        text-align: center;
        .count{
          font-size: 10px;
        }
        .icon-keyboard_arrow_right{
          line-height: 24px;
          font-size: 10px;
        }
      }

    }
    .bulletin-wrapper {
      display: flex;
      width:100%;
      height: 28px;
      padding: 0 0 0 12px;
      background: rgba(0,0,0,0.2);
      line-height: 28px;
      .bulletin-title {
        display: inline-block;
        vertical-align:middle;
        margin-top: 8px;
        width: 22px;
        height: 12px;
        background: url(bulletin@2x.png);
        background-size: 22px 12px;
      }
      .bulletin-text {
        flex: 1;
        vertical-align: top;
        margin: 0 4px;
        font-size: 10px;
        color: #fff;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }
      .icon-keyboard_arrow_right{
        color: #fff;
        line-height: 28px;
        margin-right: 16px;
      }
    }
    .background{
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }

    .detail{
      background: rgba(7,17,27,0.8);
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 20;
      overflow: auto;
      &.slide-enter-active,&.slide-leave-active{
        transition: all ease .4s;
        opacity: 1;
      }
      &.slide-enter,&slide-leave-to{
        transition: all ease .4s;
        opacity: 0;
      }
      .detail-wrapper{
        width: 100%;
        height: 100%;
        .detail-main{

          margin: 64px 36px 32px 36px;
          .name{
            font-size: 16px;
            color: #fff;
            font-weight: 700;
            line-height: 16px;
            text-align: center;
          }
          .start-wrapper{
            margin-top: 18px;
            height: 24px;
            margin-bottom: 28px;
            text-align: center;
          }
          .title{
            position: relative;
            margin-bottom: 24px;
            .line1{
              &::before{
                width: 112px;
                height: 1px;
                content: "";
                position: absolute;
                top: 50%;
                left: 0px;
                background: rgba(255,255,255,0.2);
              }
            }
            .name{
              font-size: 14px;
              font-weight: 600;line-height: 14px;
            }
            .line2{
              &::after{
                width: 112px;
                height: 1px;
                content: "";
                display: block;
                position: absolute;
                top: 50%;
                right: 0px;
                background: rgba(255,255,255,0.2);
              }
            }
          }
          .supports{
            margin: 0 12px 12px 12px;
            overflow: hidden;
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
              margin-left: 6px;
            }
          }
          .notice{
            position: relative;
            margin-bottom: 24px;
            margin-top: 16px;
            .line1{
              &::before{
                width: 112px;
                height: 1px;
                content: "";
                position: absolute;
                top: 50%;
                left: 0px;
                background: rgba(255,255,255,0.2);
              }
            }
            .name{
              font-size: 14px;
              font-weight: 600;line-height: 14px;
            }
            .line2{
              &::after{
                width: 112px;
                height: 1px;
                content: "";
                display: block;
                position: absolute;
                top: 50%;
                right: 0px;
                background: rgba(255,255,255,0.2);
              }
            }
          }
          .notice-text{
            margin: 0 12px;
            font-size: 12px;
            font-weight: 200;
            line-height: 24px;
            color: rgba(255,255,255,0.8);
          }
          .detail-close{
            width: 32px;
            height: 32px;
            font-size: 32px;
            color: rgba(255,255,255,0.5);
            position: relative;
            margin: 50px auto 0 auto;
            clear: both;
          }
        }
      }
      .clearfix{
        display: inline-block;
        &:after{
          display: block;
          content: "";
          height: 0;
          line-height: 0;
          clear: both;
        }
      }
    }
  }
</style>
