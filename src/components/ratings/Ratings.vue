<template>
  <div  v-if="seller!=null&&ratings!=null" class="bigbox" ref="ratingsWrapper">
    <div  >
      <div >
        <div class="score">
          <div class="score_left">
            <h2>{{seller.score}}</h2>
            <h3>综合评分</h3>
            <p>高于周边商家</p>
          </div>
          <ul class="score_right" >
            <li >
              <span>服务态度 </span>
              <div class="star"><star :score="seller.serviceScore" :width="24"></star></div>
              <b> {{seller.serviceScore}}</b>
            </li>
            <li>
              <span>美食评分</span>
              <div class="star"><star :score="seller.foodScore" :width="24"></star></div>
              <b> {{seller.foodScore}}</b>
            </li>
            <li>送达时间 <i>{{seller.deliveryTime}}分钟</i></li>
          </ul>
          <div style="clear: both"></div>
        </div>
        <div class="pad_bg"></div>
        <ul class="rating_satisfiy" >
          <li @click="type=2">全部</li>
          <li @click="type=0">满意</li>
          <li @click="type=1">不满意</li>
        </ul>
        <div class="look_ratings">
          <h5><i class="icon-check_circle" @click="isContent" :class="{show_con:conFlag}"></i>只看有内容的评价</h5>
          <div class="ratings_content">
            <ul >
              <li v-for="rating in ratings"  v-if="rating.rateType==type||type==2" v-show="rating.show==true">
                <img :src="rating.avatar" alt="">
                <div class="rating_right">
                  <b>{{rating.username}}</b>
                  <div class="xingxinghang" style="display:table">
                    <div class="xingxing_ratingcontent">
                      <star :score="rating.score" :width="24" ></star></div>
                    <p style="display:table-cell">{{rating.deliveryTime}}分钟送达</p>
                  </div>
                  <h4 >{{rating.text}}</h4>
                  <div class="tuijianhang">
                    <i @click="rating.liang=!rating.liang" class="icon-thumb_up  up "
                       :class="{dianliang:rating.liang}" v-if="rating.rateType==0"></i>
                    <i @click="rating.liang=!rating.liang" :class="{dianliang:rating.liang}"
                       class="icon-thumb_down  down" v-if="rating.rateType==1"></i>
                    <p><span class="tuijian" v-for="tuijian  in rating.recommend">{{tuijian}}</span></p>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Star from '../star/Star.vue';
  import axios from "axios";
  import BetterScroll from 'better-scroll';
  import Vue from "vue";
  export default{
    props:['ratings'],
    data(){
      return {
        seller:null,
//        ratings:null,
        ratingsScroll:null,
        type:2,
        conFlag:false

      }
    },
    components:{
      Star
    },
    methods:{
      isZan(){
        let zan='';
        if(this.ratings.rateType==0){
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

      }
    },
    computed:{

    },
    created(){
      let that=this;
      /*http://localhost:8080/api/seller*/
//      /api/seller
      axios.get('/api/seller').then((res)=>{
        that.seller=res.data.data;
        this.$nextTick(()=>{  //保证数据更新后引起的DOM更新之后会调用
          this.ratingsScroll = new BetterScroll(this.$refs.ratingsWrapper, {
            click: true,
          });
        });
      }).catch((error)=>{
        console.log(error);
      });

      that.ratings.forEach((rating)=>{
        Vue.set(rating,"liang",false);
        Vue.set(rating,"show",true);
      });


    }
  }
</script>

<style>
  .bigbox{
    position: absolute;
    top:3.5rem;
    bottom:1rem;
    overflow: hidden;
  }
  .score{
    width:100%;
    height:1.6rem;
    padding: 0.36rem 0;
    position: relative;
    display: flex;
  }
  .score .score_left{
    width:2.75rem;
    border-right:1px solid #aaa ;
    text-align: center;
    flex:0 0 2.75rem;
  }
  .score .score_left h2{
    font-size:0.48rem;
    color: rgb(255,153,0);
    line-height:0.56rem;
  }
  .score .score_left h3{
    margin-top:0.12rem;
    margin-bottom:0.16rem;
    font-size: 0.24rem;
    line-height: 0.24rem;
  }
  .score .score_left p{
    font-size:0.2rem;
    color: rgb(147,153,159);
    line-height:0.2rem;
    margin-bottom: 0.36rem;

  }
  .score .score_right{
    width:60%;
    flex:1;
  }
  .score .score_right p{
    margin-left:0.3rem;
    font-size:0.24rem;
    line-height: 0.36rem;
    color: rgb(7,17,27);
  }
  .score_right li{
    display: table;
    height:0.5rem;
    margin-left:0.3rem;
    line-height:0.5rem;
    vertical-align: middle;
  }
  .score_right li span{
    display: table-cell;
    margin-right:0.24rem;
  }
  .score_right li .star{
    margin-left:0.2rem;
  }
  .score_right li b{
    display: table-cell;
    color: #ff9900;

  }

  .pad_bg{
    width:100%;
    height:0.2rem;
    background: #f3f5f7;
    border-bottom: 1px solid #e6e7e8;
    border-top: 1px solid #e6e7e8;
  }
  .rating_satisfiy{
    padding-bottom:0.36rem;
    margin:0.36rem;
    height:0.64rem;
    border-bottom: 1px solid #e6e7e8;
    text-align:center;

  }
  .rating_satisfiy li{
    width:1.22rem;
    height:0.64rem;
    background: #00a0dc;
    float: left;
    line-height:0.64rem;
    margin-right: 0.2rem;
    color: #fff;
  }
  .rating_satisfiy li:nth-child(2){
    background: #ccecf8;
    color: #4d555d;
  }
  .rating_satisfiy li:nth-child(3){
    background: #e9ebec;
    color: #4d555d;
  }
  .look_ratings h5{
    width:100%;
    height:0.7rem;
    border-bottom: 1px solid #e6e7e8;
    padding-left:0.36rem;
    color: #93999f;
  }
  .look_ratings h5 i{
    font-size:0.5rem;
    vertical-align: middle;
  }
  .look_ratings h5 i.show_con{
    color:rgb(0,160,220);
  }
  .look_ratings .ratings_content {
    margin-left:0.36rem;
    margin-right:0.36rem;
    border-bottom:1px solid #e6e7e8;
  }
  .look_ratings .ratings_content ul{
  }
  .look_ratings .ratings_content li{
    height:2.63rem;
    padding-top:0.36rem;

  }
  .look_ratings .ratings_content li img{
    width:0.56rem;
    height:0.56rem;
    -webkit-border-radius:50%;
    -moz-border-radius:50%;
    border-radius:50%;
    display: block;
    float: left;
  }
  .look_ratings .ratings_content li .rating_right{
    margin-left:1rem;
    margin-right:0.36rem;
    line-height: 0.32rem;
  }
  .look_ratings .ratings_content li .rating_right b{
    font-size:0.2rem;
    color: rgb(7,17,27);
    line-height:0.24rem;
  }
  .look_ratings .ratings_content li .rating_right .xingxinghang{
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
  }
  .look_ratings .ratings_content li .rating_right h4{
    font-size:0.24rem;
    color: rgb(7,17,27);
    margin-bottom:0.16rem;
  }
  .look_ratings .ratings_content li .rating_right .tuijianhang{
    line-height: 0.32rem;
    display: table;
  }
  .look_ratings .ratings_content li .rating_right .up{
    font-size:0.24rem;
    color:#b7bbbf;
    width: 0.32rem;
    height:0.32rem;
    display: table-cell;
    vertical-align: middle;
  }
  .look_ratings .ratings_content li .rating_right .down{
    font-size:0.24rem;
    color:#b7bbbf;
    display: inline-block;
  }
  .look_ratings .ratings_content li .rating_right .dianliang{
    color: rgb(0,160,220);
  }
  .look_ratings .ratings_content li .rating_right p{
    height:0.4rem;
    overflow: hidden;
    box-sizing: border-box;
  }
  .look_ratings .ratings_content li .rating_right span.tuijian{
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






</style>
