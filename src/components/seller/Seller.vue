<template>
  <div v-if="seller!=null" ref="sellerWrapper" class="seller_box">
    <div >
     <div class="seller_head">
       <h2>{{seller.name}}</h2>
       <div class="seller_pingxinghang">
       <div class="seller_star"><star :score="seller.score" :width="36"></star></div>
       <p>({{parseInt(seller.score/5*seller.sellCount)}})</p>
       <span>月售{{seller.sellCount}}单</span>
       </div>
       <div class="seller_collect">
            <i class="icon-favorite love" :class="{love_true:flag}"
            @click="flag=!flag"></i>
            <p v-show="flag==false">收藏</p>
            <p v-show="flag==true">已收藏</p>
       </div>
     </div>
      <ul class="seller_peisong">
        <li>
          <p>起送价</p>
          <b>{{seller.minPrice}} <i>元</i></b>
        </li>
        <li>
          <p>商家配送</p>
          <b>{{seller.deliveryPrice}}<i>元</i></b>
        </li>
        <li>
          <p>平均配送时间</p>
          <b>{{seller.deliveryTime}}<i>分钟</i></b>
        </li>
      </ul>
      <div class="seller_bg"></div>
      <div class="seller_gonggao">
          <h2>公告与活动</h2>
          <p>{{seller.bulletin}}</p>
      </div>
      <ul class="seller_youhui" >
        <li v-for="support in seller.supports"><i class="icon" :class="seller_picType[support.type]" style="display:
          inline-block"></i>{{support.description}}</li>
      </ul>
      <div class="seller_bg"></div>
      <!--seller_shijing-->
      <div class="seller_shijing" >
        <h2>商家实景</h2>
          <div ref="shijingWrapper" class="shijing_box">
            <ul >
              <li v-for="pic in seller.pics">
                <img :src="pic" alt="">
              </li>
            </ul>
          </div>
      </div>
      <!--seller_shijing end-->
      <!--seller_info-->
      <div class="seller_info">
        <h2>商家信息</h2>
        <ul>
          <li v-for="info in seller.infos" class="info">{{info}}</li>
        </ul>
      </div>
      <!--seller_info end-->
    </div>
  </div>
</template>
<script>
  import axios from 'axios';
  import Star from '../star/Star.vue';
  import BetterScoll from 'better-scroll';
  export default{
      props:['seller'],
      data(){
          return {
//            seller:null,
            sellerScroll:null,
            shijingScroll:null,
            seller_picType:[],
            flag:false
          }
      },
    components:{
         Star
    },
      created(){
          let that=this;
        that.$nextTick(()=>{
          that.sellerScroll=new BetterScoll(that.$refs.sellerWrapper,{
          click:true
        });
        that.shijingScroll=new BetterScoll(that.$refs.shijingWrapper,{
          click:true,
          /*横向滚动要加上*/
          scrollX:true,
          eventPassthrough:'vertical'
          /*横向滚动要加上*/
        })
      });






          this.seller_picType=['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      }
  }
</script>
<style>
  .seller_box{
    position: absolute;
    top:3.5rem;
    bottom:1rem;
    overflow: hidden;
  }
  .seller_head{
    height:1.1rem;
    margin: 0.36rem 0.36rem 0 0.36rem;
    border-bottom:1px solid rgba(7,17,27,0.1);
    position: relative;
  }
  .seller_head h2{
    color: rgb(7,17,27);
    font-size:0.28rem;
    line-height:0.28rem;
  }
  .seller_head .seller_pingxinghang{
    margin-top:0.16rem;
    display: table;
  }
  .seller_head .seller_pingxinghang .seller_star{
    display: table-cell;
    vertical-align: middle;
    display: inline-block;
    /*color: rgb(77,85,93);*/
  }
  .seller_head .seller_pingxinghang p{
    display: table-cell;
    vertical-align: middle;
    display: inline-block;
  }
  .seller_head .seller_pingxinghang span{
    display: table-cell;
    vertical-align: middle;
    display: inline-block;
  }


  .seller_head .seller_pingxinghang p{
   margin: 0 0.14rem 0 0.06rem;
    font-size: 0.2rem;
  }
  .seller_head .seller_collect{
    width: 1rem;
    height: 0.8rem;
    text-align: center;
    position: absolute;
    right:0;
    top:0;
  }
  .seller_head .seller_collect .icon-favorite.love{
    font-size:0.48rem;
   color:rgba(0,0,0,0.4);
    line-height:0.48rem;
    display: block;
  }
  .seller_head .seller_collect .icon-favorite.love.love_true{
    color: rgb(240,20,20);
  }
  .seller_head .seller_collect p{
    margin-top:0.14rem;
    font-size:0.2rem;
    color:rgb(77,85,93);
    line-height: 0.2rem;
  }
  .seller_peisong{
    height:1.1rem;
    border-bottom:1px solid rgba(7,17,27,0.1);
    padding:0.36rem 0 ;
    display: flex;
    text-align: center;
  }
  .seller_peisong li{
    flex:1;
    border-right:1px solid gray;
  }
  .seller_peisong li:last-child{
    border: 0;
  }
  .seller_peisong li p{
    font-size: 0.2rem;
    color: rgb(147,153,159);
    line-height:0.2rem;
    margin-bottom: 8px;
  }
  .seller_peisong li b{
    display: block;
    font-size:0.48rem;
    font-weight: 200;
    color: rgb(7,17,27);
    line-height:0.48rem;
  }
  .seller_peisong li b i{
    font-size: 0.2rem;
  }
  .seller_bg{
    background: #f3f5f7;
    height:0.34rem;
  }
  .seller_gonggao{
    height: 3.1rem;
    border-top:1px solid rgba(7,17,27,0.1);
    border-bottom:1px solid rgba(7,17,27,0.1);
    padding: 0.36rem;
  }
  .seller_gonggao h2{
    color:#07111b;
    margin-bottom:0.16rem;
    font-size:0.36rem;
  }
  .seller_gonggao p{
    font-size:0.24rem;
    font-weight:200;
    color: rgb(240,20,20);
    line-height:0.48rem;
    padding: 0 0.24rem;
  }
  .seller_youhui ul{

  }
  .seller_youhui li{
    margin-left:0.6rem;
    margin-right:0.6rem;
    height:0.96rem;
    font-size: 0.24rem;
    color: rgb(7,17,27);
    line-height:0.96rem;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .seller_youhui li:last-child{
    margin:0;
    padding:0 0.6rem;
  }
  .seller_youhui .icon{
    width:0.32rem;
    height:0.32rem;
    display: inline-block;
    background-size: 0.32rem 0.32rem;
    margin-right:0.2rem;
    vertical-align: middle;
  }
  .seller_youhui .icon.decrease{
    background-image: url('images/decrease_1@2x.png') ;
  }
  .seller_youhui .icon.discount{
    background-image: url('images/discount_1@2x.png');
  }
  .seller_youhui .icon.guarantee {
    background-image: url('images/guarantee_1@2x.png');
  }
  .seller_youhui .icon.invoice {
    background-image: url('images/invoice_1@2x.png');
  }
  .seller_youhui .icon.special {
    background-image: url('images/special_1@2x.png');
  }
  .seller_shijing{
    padding: 0.36rem 0 0.36rem 0.36rem;
    border-top:1px solid rgba(7,17,27,0.1);
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .seller_shijing h2{
    color: #07111b;
    margin-bottom: 0.16rem;
    font-size: 0.36rem;
  }


  .seller_shijing ul{
    width:150%;
    height:1.8rem;
  }
  .seller_shijing ul li{
    width:2.4rem;
    height: 1.8rem;
    float: left;
    margin-right:0.12rem;
  }
  .seller_shijing ul li img{
    width:2.4rem;
    height: 1.8rem;
  }
  .seller_info{
    padding:0.36rem;
  }
  .seller_info h2{
    color: #07111b;
    padding-bottom: 0.16rem;
    font-size: 0.36rem;
    border-bottom: 1px solid  rgba(7,17,27,0.1);
  }
  .seller_info .info{
    margin: 0.32rem 0.24rem;
    font-size: 0.24rem;
    color: rgba(7,17,27,0.8);
    line-height: 0.32rem;

  }



</style>
