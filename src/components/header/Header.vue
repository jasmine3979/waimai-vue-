<template>
  <div class="header_all">
    <!--模糊背景-->
    <div class="bg">
      <img :src="imginfo.avatar" alt="" class="imgbg">
      <div class="mengban"></div>
    </div>
    <div class="header">
      <div class="header_l">
        <img :src="imginfo.avatar" alt="" class="goodimg">
      </div>
      <div class="header_r">
         <img src="./images/brand@2x.png" alt="" class="brand">
          <p >{{imginfo.name}}</p>
          <b >{{imginfo.description}} / {{imginfo.deliveryTime}}分钟送达</b>
          <div class="support"  v-if="imginfo.supports">
            <span class="icon decrease" ></span>
            <span class="text" >{{imginfo.supports[0].description}}</span>
          </div>
      </div>
      <!--5个-->
      <div class="support-count" v-if="imginfo.supports" @click="detailShow=true">
        <span class="count">{{imginfo.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="detailShow=true">
      <span class="bulletin-title"></span>
      <p class="bulletin-text">{{imginfo.bulletin}}</p>
      <i class="icon-keyboard_arrow_right"></i>
    </div>


    <!--详情弹层-->
    <div class="detail" v-if="detailShow">
      <div class="detail-wrapper clearfix"> <!--css sticker footers的固定套路-->
        <div class="detail-main">
          <h1 class="name">{{imginfo.name}}</h1>
          <div class="star-wrapper">
            <!--星级组件-->
            <star :score="imginfo.score" :width="48"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul  class="supports">
            <li class="support-item"  v-for="support in imginfo.supports">
              <span class="icon" :class="picMap[support.type]"></span>
              <span class="text">{{support.description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{imginfo.bulletin}}</p>
          </div>

          <!--<div class="bulletin">
            <p class="content">{{imginfo.bulletin}}</p>
          </div>
          <div class="bulletin">
            <p class="content">{{imginfo.bulletin}}</p>
          </div>-->

        </div>
      </div>
      <div class="detail-close"  @click="detailShow=false">
        <span class="icon-close"></span>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import Star from '../star/Star.vue'
  export default{
      data(){
          return {
            imginfo:{},
            picMap:[],
            detailShow:false,
          }
      },
    created(){
          var that=this;
          /*http://localhost:8080/api/seller*/
//      /api/seller
          let url='/api/seller';
          axios.get(url).then((res)=>{
              that.imginfo=res.data.data;
          }).catch((error)=>{
              console.log(error);
          });


      this.picMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
    },
    components:{
        Star
    }
  }
</script>

<style>
  .header_all{
    position: relative;
    /*z-index:100;*/
    background: #fff;
  }
  .header{
    height: 2.12rem;
    position: relative;
    background: rgba(7,17,27,0.5);
  }
  .bg{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    filter: blur(10px);

  }

  .bg .imgbg{
    width:100%;
    height:100%;
  }
  .bg .mengban{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: rgba(0,0,0,0.2);
  }



  .header_l{
    width:2.08rem;
    height:2.12rem;
    float: left;
  }
  .goodimg{
    width:1.28rem;
    height:1.28rem;
    margin-top:0.48rem ;
    margin-left:0.48rem;
    border-radius: 0.04rem;
  }

  .header_r{
    float: left;
    width:50%;
    padding-top: 0.52rem;
    height:1.28rem;
  }
  .header_r .brand{
    height:0.32rem;
    float: left;
    margin-right: 0.12rem;

  }
  .header_r p{
    font-size:0.32rem;
    color: rgb(255,255,255);
    font-weight:bold;
    line-height:0.36rem;
    margin-left: 0.12rem;

  }
  .header_r b{
    display: block;
    font-size: 0.24rem;
    color:rgb(255,255,255);
    font-weight: 200;
    line-height: 0.24rem;
    margin-top: 0.16rem;
    margin-bottom:0.2rem;
  }
  .header_r .support{
    font-size:0.2rem;
    color: rgb(255,255,255);
    font-weight: 200;
    line-height: 0.24rem;
  }
  .header_r .icon{
    width: 0.24rem;
    height: 0.24rem;
    margin-right: 0.08rem;
    display: inline-block;

    vertical-align: top;

  }

  /*五种小图标*/
  .header_r .icon.decrease{
    background: url('./images/decrease_1@2x.png')no-repeat;
    background-size: 0.24rem 0.24rem;
  }
  .header_r .icon.discount{
    background: url('./images/discount_1@2x.png')no-repeat;
    background-size: 0.24rem 0.24rem;
  }
  .header_r .icon.guarantee{
    background: url('./images/guarantee_1@2x.png')no-repeat;
    background-size: 0.24rem 0.24rem;
  }
  .header_r .icon.invoice{
    background: url('./images/invoice_1@2x.png')no-repeat;
    background-size: 0.24rem 0.24rem;
  }
  .header_r .icon.special{
    background: url('./images/special_1@2x.png')no-repeat;
    background-size: 0.24rem 0.24rem;
  }

  .support-count{
    padding: 0 0.16rem;
    height: 0.48rem;
    background: rgba(0,0,0,0.2);
    font-size: 0.2rem;
    color: rgb(255,255,255);
    font-weight: 200;
    line-height: 0.48rem;
    float: right;
    border-radius: 14px;
    text-align: center;
    position: absolute;
    bottom: 18px;
    right: 12px;
  }
  .bulletin-wrapper{
    position: relative;
    width:100%;
    height:0.56rem;
    line-height: 0.56rem;
    color: rgb(255,255,255);
    background: rgba(7,17,27,0.7);
  }

  .bulletin-wrapper .bulletin-title{
    width:0.44rem;
    height:0.24rem;
    display: block;
     margin: 0.14rem 0.08rem 0 0.24rem;
    float: left;
    vertical-align: top;
    background: url("images/bulletin@2x.png" ) no-repeat;
    background-size: 0.44rem 0.24rem;
  }
  .bulletin-wrapper .bulletin-text{
    width: 85%;
    float: left;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .bulletin-wrapper .icon-keyboard_arrow_right{
    float: left;
    line-height: 0.56rem;
  }

  /*弹出层*/
  .detail{
    min-height: 100%;
    width:100%;
    position:fixed;
    z-index:1000;
    left:0;
    top:0;
    background: rgba(7, 17, 27, .8);
    color: rgb(255,255,255);
  }

  .detail-wrapper{
    width:100%;
    padding-bottom:0.64rem;
  }
  .detail-main{
    margin-top:1.28rem;
    padding-bottom: 1.28rem;
  }

  .name{
    line-height: 0.32rem;
    text-align: center;
    font-size: 0.32rem;
    font-weight: 700;
  }

  .star-wrapper{
    margin-top: 0.36rem;
    padding: 0.04rem 0;
    text-align: center;
  }
  .title {
    display: flex;
    width: 80%;
    margin: 0.56rem auto 0.48rem auto;
  }
  .line{
    flex: 1;
    position: relative;
    top: -0.12rem;
    border-bottom: 1px solid rgba(255, 255, 255, .2);
  }

  .text{
    padding: 0 0.24rem;
    font-size: 0.28rem;
    font-weight: 700;
  }

  .supports {
    width: 80%;
    margin: 0 auto;
  }
  .support-item {
    padding: 0 0.24rem;
    margin-bottom: 0.24rem;
    font-size: 0;
  }

  .supports .support-item .icon{
    display: inline-block;
    width: 0.32rem;
    height: 0.32rem;
    margin-right: 0.12rem;
    background-size:0.32rem;
    vertical-align: top;
  }
  .supports .support-item .icon.decrease {
    background-image: url('images/decrease_1@2x.png');
  }
  .supports .support-item .icon.discount {
    background-image: url('images/discount_1@2x.png');
  }
  .supports .support-item .icon.guarantee {
    background-image: url('images/guarantee_1@2x.png');
  }
  .supports .support-item .icon.invoice {
    background-image: url('images/invoice_1@2x.png');
  }
  .supports .support-item .icon.special {
    background-image: url('images/special_1@2x.png');
  }

  .supports .support-item .text{
    line-height:0.32rem;
    font-size: 0.24rem;
  }


  .detail .bulletin{
    width: 80%;
    margin: 0 auto;
  }

  .detail .content{
    padding: 0 0.24rem;
    line-height: 0.48rem;
    font-size: 0.24rem;
  }
  .detail:after {
     display: block;
     content: "";
     height: 0;
     line-height: 0;
     clear: both;
   }

  .detail-close {


    width:100%;
    color: #fff;
    font-size:0.64rem;
   text-align: center;

    position: relative;
    margin-top: -0.44rem;  /* 使footer区块正好处于content的padding-bottom位置 */
    height: 50px;
    clear: both;

  }




</style>



