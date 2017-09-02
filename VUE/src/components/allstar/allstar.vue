<template xmlns="http://www.w3.org/1999/html">
  <div class="allStar">
    <div :class="{'star1':pattern==1}" v-if="Math.floor(msg)>=1 && pattern==1">

    </div>
    <div :class="{'star1':pattern==1}" v-if="Math.floor(msg)>=2 && pattern==1">

    </div>
    <div :class="{'star1':pattern==1}" v-if="Math.floor(msg)>=3 && pattern==1">

    </div>
    <div :class="{'star1':pattern==1}" v-if="Math.floor(msg)>=4 && pattern==1">

    </div>
    <div :class="{'star1':pattern==1}" v-if="Math.floor(msg)>=5 && pattern==1">

    </div>
    <div :class="{'star2':pattern==1}" v-if="msg%1<0.5 && pattern==1">

    </div>
    <div :class="{'star3':pattern==1}" v-if="msg%1>=0.5 && pattern==1">

    </div>
    <!--{{pattern}}-->
  </div>
</template>

<script>
    export default{
      props:["msg","pattern"],
        data(){
          return {
              name:"test",
          }
        },

        methods:{
          sendData:function () {
            console.log("sendData方法……开始发送receive事件");
            this.$emit("receive",this.name);
          }
        }
    }
</script>

<style lang="less" rel="stylesheet/less">
  .allStar{

    .star1{
      display: inline-block;
      width: 20px;
      height: 20px;
      background: url("star24_on@2x.png");
    }
    .star2{
      display: inline-block;
      width: 20px;
      height: 20px;
      background: url("star24_off@2x.png");
    }
    .star3{
      display: inline-block;
      width: 20px;
      height: 20px;
      background: url("star24_half@2x.png");
    }
  }
</style>
