<template>
    <div class="goods"  v-if="goods!=null">
      <!--左边-->
      <div class="goods_left" ref="menuWrapper">
        <div style="height: 130%">
        <div v-for="(good,index) in goods" @click="selel(index,$event)"
            class="good_item" :class="{bgcolor:setClass===index}">
          <div class="cc">
            <div class="smallbox">
             <i class="xiaotubiao" v-if="good.type>0" :class="picMap[good.type]"></i>
              <b>{{good.name}}</b>
            </div>
          </div>
        </div>
        </div>
      </div>
      <!--右边-->
      <div class="goods_right" ref="goodsWrapper">
        <div >
          <div v-for="good in goods"  class="goods-item item-hook">
              <p class="good-name">{{good.name}}</p>
            <!--每一项事物-->
              <div v-for="food in good.foods" class="food-item clearfix" >
                <img :src="food.icon" width="57" height="57" class="food_img" @click="selectFood(food)">
                  <div class="food_detail" >
                  <h2 class="name">{{food.name}}</h2>
                  <h5 v-show="food.description!=null">{{food.description}}</h5>
                  <p><b>月售{{food.sellCount}}</b><span>好评率{{food.rating}}%</span></p>
                  <p><span class="price1">￥{{food.price}}</span><i class="old_price" v-if="food.oldPrice">￥{{food
                    .oldPrice}}</i></p>
                   <div class="line"></div>
                    <!--加入或减少数量的容器-->
                    <div class="cartopp-wrapper" >
                      <!--加减购物车组件-->
                      <cartopp :food="food"></cartopp>
                    </div>

                </div>
              </div>

          </div>
        </div>
      </div>

      <!--每一项食物组件-->
      <div v-if="foodItem!=null" v-show="foodItemShow">
        <food :food="foodItem" @receiveMsg="receive"></food>
      </div>
    </div>
</template>

<script>
  import axios from 'axios';
  import BetterScroll from 'better-scroll';
  import Cartopp from '../cartopp/Cartopp.vue';
  import Shopcart from '../shopcart/Shopcart.vue';
  import Food from "../food/Food.vue";
  export default{
      props:['goods'],
      data(){
          return{
               picMap:[],
              menuScroll: null,
              goodsScroll: null,
              scrollY: 0,
              foodItem:null,
              foodItemShow:true,
              listHeight:[]
          }
      },
    methods:{
      receive(flag){
        this.foodItemShow=flag;
      },
      selectFood(food){
        this.foodItem=food;
        this.foodItemShow=true;
      },
      selel(index,event){
        console.log(event);
        if(!event._constructed){
            return false;
        }else{
            let lis=this.$refs.goodsWrapper.getElementsByClassName('item-hook');
            let ele=lis[index];
          this.goodsScroll.scrollToElement(ele,300);
          console.log(this.goodsScroll);
        }
      },
      getHeight(){
          let height=0;
          this.listHeight.push(height);
          let lis=this.$refs.goodsWrapper.getElementsByClassName('item-hook');
         for(let i=0;i<lis.length;i++){
             height+=lis[i].clientHeight;
             this.listHeight.push(height);
         }
         console.log(this.listHeight);
      }
    },
    components:{
          Cartopp,
          Shopcart,
          Food,
    },

      created(){
            this.$nextTick(()=>{  //保证数据更新后引起的DOM更新之后会调用
              this.menuScroll = new BetterScroll(this.$refs.menuWrapper, {
                click: true,
              });
              this.goodsScroll = new BetterScroll(this.$refs.goodsWrapper, {
                click: true,
                probeType: 3
              });
//              console.log(this.goodsScroll);
              this.goodsScroll.on("scroll", (pos)=>{
                this.scrollY = Math.abs(Math.round(pos.y));
              });
              //计算高度
              this.getHeight();
            });

        this.picMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      },
    computed:{
      setClass(){
          for(let i=0;i<this.listHeight.length;i++){
              let height=this.listHeight[i];
              let height1=this.listHeight[i+1];
              if(!height1||(this.scrollY>=height&&this.scrollY<height1)){
                  return i;
              }
          };
          return 0;
      }
    }
  }
</script>

<style>
    .goods{
      position: absolute;
      top:3.48rem;
      bottom:1rem;
      font-size: 0.24rem;
    }
    .goods  .goods_left{
      width:22%;
      float: left;
      overflow: hidden;
      height: 100%;
    }
    .goods .goods_left div.cc{
      font-size:0;
      width:1.6rem;
      height:1.08rem;
      position: relative;
      /*纵向居中办法*/
      /*display: flex;
      justify-content: center;
      flex-direction: column;
      text-align: center;*/
      /*display: table;*/

    }
    .goods .goods_left div.cc .smallbox{
      position: absolute;
      top: 50%;
      left:10%;
      right:10%;
      transform: translateY(-50%);

    }
    .goods .goods_left div.cc i.xiaotubiao{
        width: 0.32rem;
        height:0.32rem;
      display: inline-block;
      vertical-align:top;

    }
    /*五种小图标*/
    .goods .goods_left div.cc i.xiaotubiao.decrease{
      background: url('./images/decrease_1@2x.png') no-repeat;
      background-size:100% 100%;
    }
    .goods .goods_left div.cc i.xiaotubiao.discount{
      background: url('./images/discount_1@2x.png') no-repeat;
      background-size:100% 100%;
    }
    .goods .goods_left div.cc i.xiaotubiao.guarantee{
      background: url('./images/guarantee_1@2x.png') no-repeat;
      background-size:100% 100%;
    }
    .goods .goods_left div.cc i.xiaotubiao.invoice{
      background: url('./images/invoice_1@2x.png') no-repeat;
      background-size:100% 100%;
    }
    .goods .goods_left div.cc i.xiaotubiao.special{
      background: url('./images/special_1@2x.png') no-repeat;
      background-size:100% 100%;
    }

    .goods .goods_left div.cc b{
      /*background: red;*/
      font-size:0.28rem;
      vertical-align: middle;
      /*display: table-cell;*/
    }


  .goods .good_item{
     background: #f3f5f7;
     border-bottom:1px solid rgba(7,17,27,0.1);
   }
  .goods .bgcolor{
      background: rgba(0,0,255,0.5);
      color: #fff;
    }

    .goods  .goods_right{
      width:78%;
      float: left;
      overflow: hidden;
      height: 100%;
    }
    .goods  .goods_right .goods-item .good-name{
        height: 0.54rem;
        width: 100%;
      line-height:0.54rem;
        background: #f3f5f7;
         color: #93999f;
    }
    .goods  .goods_right .goods-item .food-item{
      width:100%;
      border-bottom: 1px solid #d9dde1;
      position:relative;
    }
    .goods  .goods_right .goods-item .food-item .food_img{
      margin:0.36rem;
      margin-right:0.2rem;
      float: left;
    }
    .goods  .goods_right .goods-item .food-item .food_detail{
      float: left;
      width: 60%;
    }
    .goods  .goods_right .goods-item .food-item .food_detail h2{
      margin-top: 0.4rem;
      margin-bottom: 0.16rem;
      text-align: left;
      font-weight: 200;
    }
    .goods  .goods_right .goods-item .food-item .food_detail h5{
      margin-bottom: 0.16rem;
      color: #b4b8bb;
    }
    .goods  .goods_right .goods-item .food-item .food_detail p{
      margin-bottom:0.16rem;
      color: #b4b8bb;
    }
    .goods  .goods_right .goods-item .food-item .food_detail p b{
      margin-right:0.24rem;
    }
    .goods  .goods_right .goods-item .food-item .food_detail p .price1{
      font-size: 0.28rem;
      color: rgb(147,153,159);
      color:rgb(240,20,20);
      font-weight:700;
      line-height:0.48rem;
      padding-left: 0;
    }
    .goods  .goods_right .goods-item .food-item .food_detail p .old_price{
      text-decoration: line-through;
    }
    .goods  .goods_right .goods-item .food-item .cartopp-wrapper{
      position: absolute;
      bottom:0.12rem;
      right:0.12rem;
    }


</style>
