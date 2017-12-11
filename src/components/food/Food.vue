<template>
    <div class="fooditem_all" v-if="food!=null" ref="fooditem_Wrapper">
      <div>
        <img :src="food.image" alt="" class="fooditem_image">
        <div class="fooditem_detail">
           <h2>{{food.name}}</h2>
          <p><b>月售{{food.sellCount}}</b><span>好评率{{food.rating}}%</span></p>
          <h4><span class="price1">￥{{food.price}}</span><i class="old_price" v-if="food.oldPrice">￥{{food
            .oldPrice}}</i></h4>
          <h5 class="fooditem_cart" @click="food_add">加入购物车</h5>
        </div>
        <div class="fooditem_bg"></div>
        <div class="fooditem_intro">
          <h2>商品介绍</h2>
          <p>{{food.info}}</p>
        </div>
        <div >
          <div class="foodrating_satisfiy">
            <h2>商品评价</h2>
            <ul>
            <li @click="type=2">全部 <span>{{food.ratings.length}}</span></li>
            <li @click="type=0">推荐 <span>{{fooditemNum1}}</span></li>
            <li @click="type=1">吐槽 <span>{{fooditemNum2}}</span></li>
          </ul>
          </div>
          <div class="foodlook_ratings">
            <h5><i class="icon-check_circle" @click="isContent" :class="{show_con:conFlag}"></i>只看有内容的评价</h5>
            <div class="foodratings_content">
              <ul >
                <li v-for="rating in ratings"  v-if="rating.rateType==type||type==2" v-show="rating.show==true">
                  <div class="foodrating_time">{{rating.rateTime | getTime}}</div>
                  <img :src="rating.avatar" alt="">
                  <div class="foodrating_right">
                    <b>{{rating.username}}</b>
                    <h4 >{{rating.text}}</h4>
                    <div class="foodtuijianhang">
                      <i @click="rating.liang=!rating.liang" class="icon-thumb_up  up "
                         :class="{dianliang:rating.liang}" v-if="rating.rateType==0"></i>
                      <i @click="rating.liang=!rating.liang" :class="{dianliang:rating.liang}"
                         class="icon-thumb_down  down" v-if="rating.rateType==1"></i>
                      <p><span class="foodtuijian" v-for="tuijian  in rating.recommend">{{tuijian}}</span></p>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <p class="food_return" @click="return_goods">返回</p>
      </div>
    </div>
</template>

<script>
  import BetterScroll from "better-scroll";
  import axios from 'axios';
  import Star from '../star/Star.vue';
  import Vue from "vue";
  export default{
    props:["food"],
    components:{
      Star
    },
    filters:{
      getTime:function(time){
        let date=new Date(time);
        let year=date.getFullYear();
        let month=date.getMonth()+1;
        let day=date.getDate();
        let hour=date.getHours();
        let minute=date.getMinutes();
        if(month<10){
          month="0"+month;
        }
        if(day<10){
          day="0"+day;
        }
        if(hour<10){
          hour="0"+hour;
        }
        if(minute<10){
          minute="0"+minute;
        }
        return year+"-"+month+"-"+day+" "+hour+":"+minute;
      }
    },
    data(){
        return {
          fooditem_Scroll:null,
          conFlag:false,
          type:2,
          ratings:null,
          flag:false
        }
    },
    computed:{
        fooditemNum1(){
            let sum=0;
            this.food.ratings.forEach(function(rating){
              if(rating.rateType==0){
                  sum++;
              }
            });
            return sum;
        },
      fooditemNum2(){
        let sum=0;
        this.food.ratings.forEach(function(rating){
          if(rating.rateType==1){
            sum++;
          }
        });
        return sum;
      }

    },
    methods:{
      isZan(){
        let zan='';
        if(this.ratings.rateType==1){
          zan="icon-thumb_up"
        }
        return zan;
      },
      isContent(){
        this.conFlag=!this.conFlag;
        if(this.conFlag==true){
          this.ratings.forEach(function(rating){
            if(rating.text==""){
              rating.show=false;
            }else{
              rating.show= true;
            }
          });
        }else{
          this.ratings.forEach(function(rating){
            rating.show= true;
          });
        }
      },
      food_add(){
        if(!this.food.count){
          Vue.set(this.food,'count',1);
        }else{
          this.food.count++;
        }
      },
      return_goods(){
          this.$emit("receiveMsg",this.flag);
      }
    },
    created(){
         this.ratings=this.food.ratings;
          this.$nextTick(()=>{  //保证数据更新后引起的DOM更新之后会调用
            this.fooditem_Scroll = new BetterScroll(this.$refs.fooditem_Wrapper, {
              click: true,
            });
          });

      this.ratings.forEach((rating)=>{
        Vue.set(rating,"liang",false);
        Vue.set(rating,"show",true);
      });
    }
  }
</script>

<style>
  .fooditem_all{
    width:100%;
    height:100%;
    position: fixed;
    background: #fff;
    top:0;
    left:0;
    bottom: 1rem;
  }
  .fooditem_all .fooditem_image{
    height:7.5rem;
  }
  .fooditem_all .fooditem_detail{
    padding: 0.36rem;
    border-bottom:1px solid rgba(7,17,27,0.1);
    position: relative;
  }
  .fooditem_all .fooditem_detail h2{
    font-size:0.28rem;
    font-weight:700;
    color: rgb(7,17,27);
    line-height:0.28rem;
  }
  .fooditem_all .fooditem_detail p{
    margin:0.16rem 0 0.36rem 0;
    font-size:0.2rem;
    color: rgb(147,153,159);
  }
  .fooditem_all .fooditem_detail p b{
    margin-right:0.26rem;
  }
  .fooditem_all .fooditem_detail h4 span{
    font-size:0.28rem;
    font-weight: 700;
    color: rgb(240,20,20);
    line-height: 0.48rem;
    margin-right:0.12rem;
  }
  .fooditem_all .fooditem_detail h4 i{
    font-size:0.2rem;
    font-weight: 700;
    color: rgb(147,153,159);
    line-height: 0.48rem;
    text-decoration: line-through;
  }
  .fooditem_all .fooditem_detail .fooditem_cart{

  }
  .fooditem_all .fooditem_bg{
    height:0.32rem;
    background: #f3f5f7;
  }
  .fooditem_all .fooditem_detail .fooditem_cart{
      position: absolute;
      bottom:0.36rem ;
      right:0.36rem;
    width:1.48rem;
    height:0.48rem;
    line-height:0.48rem;
    border-radius:0.24rem;
    padding: 0.12rem;
    background: rgb(0,160,220);
    color: #fff;
  }
  .fooditem_all .fooditem_intro{
    padding: 0.36rem;
    border-bottom:1px solid rgba(7,17,27,0.1);
    border-top:1px solid rgba(7,17,27,0.1);
  }
  .fooditem_all .fooditem_intro h2{
    font-size:0.36rem;
    margin-bottom:0.16rem;
  }
  .fooditem_all .fooditem_intro p{
    padding:0 0.16rem 0 0.16rem;
    font-size:0.24rem;
    color: rgb(77,85,93);
    line-height:0.48rem;
  }



  .foodrating_satisfiy{
    padding-bottom:0.36rem;
    margin:0.36rem;
    height:1.34rem;
    border-bottom: 1px solid #e6e7e8;

  }
  .foodrating_satisfiy h2{
    font-size:0.36rem;
    margin-bottom:0.36rem;
    text-align: left;
  }
  .foodrating_satisfiy li{
    text-align:center;
    width:1.22rem;
    height:0.64rem;
    background: #00a0dc;
    float: left;
    line-height:0.64rem;
    margin-right: 0.2rem;
    color: #fff;
  }
  .foodrating_satisfiy li:nth-child(2){
    background: #ccecf8;
    color: #4d555d;
  }
  .rating_satisfiy li:nth-child(3){
    background: #e9ebec;
    color: #4d555d;
  }

  .foodlook_ratings h5{
    width:100%;
    height:0.7rem;
    border-bottom: 1px solid #e6e7e8;
    padding-left:0.36rem;
    color: #93999f;
    margin-top:0.6rem;
  }
  .foodlook_ratings h5 i{
    font-size:0.5rem;
    vertical-align: middle;
  }
  .foodlook_ratings h5 i.show_con{
    color:rgb(0,160,220);
  }
  .foodlook_ratings .foodratings_content {
    margin-left:0.36rem;
    margin-right:0.36rem;
    border-bottom:1px solid #e6e7e8;
    padding-bottom:1rem ;

  }
  .foodlook_ratings .foodratings_content ul{
  }
  .foodlook_ratings .foodratings_content li{
    height:1.6rem;
    padding-top:0.36rem;
    position: relative;
  }
  .foodlook_ratings .foodratings_content li .foodrating_time{
   /* position: absolute;
    left: 0.24rem;
    top:0.24rem;*/
  }
  .foodlook_ratings .foodratings_content li img{
    width:0.56rem;
    height:0.56rem;
    -webkit-border-radius:50%;
    -moz-border-radius:50%;
    border-radius:50%;
    display: block;
    position: absolute;
    right:0.36rem;
    top:0.36rem;

  }
  .foodlook_ratings .foodratings_content li .foodrating_right{
    margin-left:1rem;
    margin-right:0.36rem;
    line-height: 0.32rem;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right b{
    font-size:0.2rem;
    color: rgb(7,17,27);
    position: absolute;
    top:0.44rem;
    right:1.2rem;
  }
  /*.look_ratings .ratings_content li .rating_right .xingxinghang{
    display: table;
    vertical-align: middle;
    margin-top:0.08rem;
    margin-bottom: 0.12rem;
  }
  .look_ratings .ratings_content li .rating_right .xingxinghang  .xingxing_ratingcontent p{
    display: table-cell;
    display: inline-block;
  }
  .look_ratings .ratings_content li .rating_right .xingxinghang p{
    color: rgb(147,153,159);
  }*/
  .foodlook_ratings .foodratings_content li .foodrating_right h4{
    font-size:0.24rem;
    color: rgb(7,17,27);
    position: absolute;
    left:0.6rem;
    top:0.8rem;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right .foodtuijianhang{
    position: absolute;
    left:0.24rem;
    top:0.8rem;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right .up{
    font-size:0.24rem;
    color:#b7bbbf;
    width: 0.32rem;
    height:0.32rem;
    display: table-cell;
    vertical-align: middle;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right .down{
    font-size:0.24rem;
    color:#b7bbbf;
    display: inline-block;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right .dianliang{
    color: rgb(0,160,220);
  }
  .foodlook_ratings .foodratings_content li .foodrating_right p{
    height:0.4rem;
    overflow: hidden;
    box-sizing: border-box;
  }
  .foodlook_ratings .foodratings_content li .foodrating_right span.tuijian{
    mix-width: 1.2rem;
    padding:0 0.12rem ;
    height:0.32rem;
    overflow: hidden;
    white-space:nowrap;
    text-overflow: ellipsis;
    padding:0 0.12rem;
    /*border:1px solid rgba(7,17,27,0.1);*/
    display: inline-block;
    margin-right:0.16rem;
    margin-bottom:0.12rem;
    color: rgb(147,153,159);
  }


  .fooditem_all .food_return{
    position: absolute;
    top: 0.16rem;
    left:0.16rem;
    color: deeppink;
    font-weight:700;
  }
</style>
