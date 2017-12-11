<template>
    <div class="cartopp">
        <div class="cart-decrease icon-remove_circle_outline" @click="sub" v-show="food.count>0"></div>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click="add"></div>

    </div>
</template>

<script>
  import Vue from 'vue';

  export default{
    props:['food'],
    methods:{
      add(){
          if(!this.food.count){
              Vue.set(this.food,'count',1);
          }else{
              this.food.count++;
          }
      },
      sub(){
          this.food.count--;
      }
    }
  }
</script>

<style>
  .cartopp{
    display: table;/*在父容器里设置*/
    text-align: center;/*在父容器里设置*/
  }
  .cartopp .cart-decrease,.cart-add{
    display: table-cell;/*在子容器里设置*/
    vertical-align: middle;/*在子容器里设置*/
    color: rgb(0, 160, 220);
    font-size: 0.48rem;
    line-height: 0.48rem;
  }

  .cartopp .cart-count{
    width:0.48rem;
    display: table-cell;
    vertical-align: middle;
    font-size: 0.24rem;
    line-height: 0.48rem;
    text-align: center;
    color: rgb(147,153,159);
    /*vertical-align: top;*/
  }
</style>
