<template>
  <div id="app" v-if="goods!=null&&ratings!=null&&seller!=null" >
    <div>
       <myheader></myheader>
    </div>
    <tabs></tabs>
      <router-view :goods="goods" :ratings="ratings" :seller="seller"></router-view>
    <!--购物车组件-->
    <shopcart :selFoods="selFoods"></shopcart>
  </div>
</template>

<script>
import myHeader from './components/header/Header.vue';
import Tabs from './components/Tabs/Tabs.vue';
import axios from 'axios';
import Shopcart from './components/shopcart/Shopcart.vue';

export default {
  name: 'app',
  data(){
      return{
          goods:null,
        ratings:null,
        seller:null
      }
  },
  components: {
    myheader:myHeader ,//前面的是标签名，后面的是引进来的名字
    Tabs,
    Shopcart
  },
  created(){
    let that=this;
    //得到goods
    /*http://localhost:8080/api/goods*/
    axios.get("/api/goods").
    then(function(res){
      that.goods=res.data.data;
    }).
    catch(function(error){
      console.log(error);
    });


    //得到ratings
    /*http://localhost:8080/api/ratings*/
    axios.get("/api/ratings").
    then(function(res){
      that.ratings=res.data.data;
    }).
    catch(function(error){
      console.log(error);
    });





    //得到seller
    /*http://localhost:8080/api/seller*/
//    /api/seller
    axios.get("/api/seller").
    then(function(res){
      that.seller=res.data.data;
    }).
    catch(function(error){
      console.log(error);
    });
  },
  computed:{
    selFoods(){
      var foods=[];
      this.goods.forEach(function(good){
        good.foods.forEach(function(food){
          if(food.count){
            foods.push(food);
          }
        })
      })
      return foods;
    },
  }
}
</script>

<style>
#app {

}

</style>
