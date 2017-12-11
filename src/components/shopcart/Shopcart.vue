<template>
  <div >
    <div class="shopcart" v-if="seller!=null" >
        <div class="content">
            <div class="content-left">
                  <div class="logo-wrapper " :class="{highlight:totalCount>0}" @click="showEvent()">
                    <b class="num" v-show="totalCount>0">{{totalCount}}</b>
                      <div class="logo " >
                        <i class="icon-shopping_cart"></i>
                      </div>
                  </div>
                  <div class="price1"><b>￥{{totalPrice}}</b> <span></span></div>
                  <div class="desc">另需配送费元￥{{seller.deliveryPrice}}元</div>
            </div>
            <div class="content-right" :class="{highlight:totalPrice>=20}" @click="spendMoney">
                   <div class="pay" >{{startSend}}</div>
            </div>
        </div>
    </div>
      <!--购物车的弹窗-->
      <div  class="shopcart_box"  v-show="selFoods.length>0&&showflag==true">

          <p class="cartbox_head"><span>购物车</span><b @click="clear">清空</b></p>
        <div ref="cartWrapper" class="cartcon_box">
          <ul>
            <li v-for="food in selFoods">
              <span>{{food.name}}</span>
                <b>￥{{food.price}}</b>
                <div class="cartbox_cartopp">
                <cartopp :food="food"></cartopp>
                </div>
            </li>
          </ul>
        </div>
      </div>
    <!--蒙版-->
    <div class="shade" v-show="showflag" v-if="selFoods.length>0"></div>
    </div>
</template>

<script>
  import axios from 'axios';
  import Cartopp from '../cartopp/Cartopp.vue';
  import BetterScroll from 'better-scroll';
  export default{
      props:['selFoods'],
    data(){
          return{
            seller:null,
            showflag:false,
            cartScroll:null
          }
    },
    created(){
       let that=this;
       /*http://localhost:8080/api/seller*/
//      /api/seller
       axios.get('/api/seller').then((res)=>{
//           console.log(res);
           that.seller=res.data.data;

       }).catch((error)=>{
           console.log(error);
       });

    },

    methods:{
        showEvent(){
          this.showflag=!this.showflag;
          this.$nextTick(function(){
            this.cartScroll=new BetterScroll(this.$refs.cartWrapper,{
              click:true,
            });
          })
        },
          spendMoney(){
              if(this.totalPrice>=20){
                  console.log("合计"+this.totalPrice);
              }
          },
          clear(){
              this.showflag=false;
              this.selFoods.forEach((food)=>{
                  food.count=0;
              })
          }
    },
    components:{
          Cartopp
    },

    computed:{
        totalCount(){
            let count=0;
            this.selFoods.forEach((food)=>{
              count+=food.count;
            });
            return count;
        },
      totalPrice(){
            let sumPrice=0;
           this.selFoods.forEach((food)=>{
             sumPrice+=food.count*food.price;
           })
        return sumPrice;
      },
      startSend(){
          let startSend="起送价"+this.seller.minPrice+"元";


          if(this.totalPrice>0){
            startSend="起送价20元";
          }
        if(this.totalPrice>=10){
          startSend=20-this.totalPrice;
          startSend="还差"+startSend+"元起送";
        }
        if(this.totalPrice>=20){
          startSend='合计';
        };
        return startSend;
      },
      zhuandong(){

      },
    }
  }
</script>


<style>
  .shopcart{
    width:100%;
    height:0.96rem;
    background: #141c27;
    position: fixed;
    bottom:0;
    left:0;
    z-index:96;
  }
  .shopcart .content{
  }
  .shopcart .content .content-left{
    width:72%;
    height:0.96rem;
    background: #141c27;
    float: left;
  }

  .shopcart .content .content-left .logo-wrapper{
    width:1.16rem;
    height:1.16rem;
    background: #2b343d;
    position: absolute;
    bottom:0.16rem;
    left:0.36rem;
    border-radius: 50%;
    border:0.12rem solid #131d26;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    z-index: 20;
  }
  .shopcart .content .content-left .highlight{
    background: #00a0dc;
  }


  .shopcart .content .content-left .logo-wrapper b{
    width:0.4rem;
    height: 0.4rem;
    position: absolute;
    right:0;
    top:-0.2rem;
    display: inline-block;
    background: red;
    color: aliceblue;
    text-align: center;
    line-height: 0.4rem;
    border-radius: 40%;
  }

  .shopcart .content .content-left .logo-wrapper .logo{
    width:100%;
    height:100%;
    border-radius: 50%;
    text-align: center;
    line-height:1.2rem;
  }

  .shopcart .content .content-left .logo-wrapper  .icon-shopping_cart{
    font-size: 0.48rem;
    color: rgba(255,255,255,0.4);
  }


  .content .content-left .price1{
    width:1.5rem;
    padding-left:1.5rem ;
    padding-top:0.24rem ;
    padding-bottom:0.24rem;
    /*border:1px solid #2c343f;*/
    height:0.96rem;
    float: left;
    font-size:0.38rem;
    color: #929397;
    position: relative;
  }
  .price1 b{display: inline-block;}
  .price1 span{
    width:0.01rem;
    height:0.6rem;
    border:1px solid #2c343f;
    /*margin-left:0.5rem;*/
    position :absolute;
    right:0.2rem;
    top:0.18rem;

  }
  .desc{
    font-size: 0.26rem;
    color: rgba(255,255,255,0.4);
    line-height:0.96rem;
  }

  .shopcart .content .content-right{
    width:28%;
    height:0.96rem;
    background: #2b343b;
    float: right;
    text-align: center;
    line-height: 0.96rem;
    color: #9c9d9f;
  }
  .shopcart .content .content-right.highlight{
    background: red;
    color: #fff;
  }
  .shopcart_box{
    width:100%;
    position: fixed;
    bottom:1rem;
    background: #fff;
    z-index: 92;
    font-size:0.26rem;
    overflow: hidden;
  }
  .shopcart_box .cartcon_box{
    max-height:4rem;
    overflow: hidden;
  }

  .shopcart_box p.cartbox_head{
    width:100%;
    height:0.82rem;
    background: #f4f5f7;
    border:1px solid #dddee0;
    line-height:0.82rem;
    font-size:0.26rem;
  }
  .shopcart_box .cartbox_head span{
    float: left;
    margin-left: 0.4rem;
    color: #292e32;
  }
  .shopcart_box .cartbox_head b{
    float: right;
    margin-right:0.3rem;
    color: #0ca2dd;
    font-weight: 400;
  }
  .shopcart_box li{
    /*width:100%;*/
    height:0.82rem;
    line-height:0.82rem;
    border:1px solid #dddee0;
    padding-left:0.4rem;
    font-size: 0.3rem;
    position: relative;
  }
  .shopcart_box li .cartbox_cartopp{
    position: absolute;
    right:0.3rem;
    top: 50%;
    margin-top:-0.2rem;
  }
  .shopcart_box li b{
    position: absolute;
    right:2rem;
    top:0;
    display: block;
    color: red;
  }


  .shade{
    position: fixed;
    width:100%;
    height:100%;
    top: 0;
    left: 0;
    background:rgba(7,17,27,0.6);
    z-index:89;
    filter: blur(10px);
  }


</style>
