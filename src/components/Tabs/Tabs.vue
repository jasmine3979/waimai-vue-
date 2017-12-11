<template>
    <div>
      <ul class="menu">
        <router-link to="/goods" tag="li">商品</router-link>
        <router-link to="/ratings" tag="li">评价</router-link>
        <router-link to="/seller" tag="li">商家</router-link>
      </ul>
    </div>
</template>

<script>
  export default{}
</script>

<style>
  .menu{
    width:100%;
    height:0.8rem;
    line-height: 0.8rem;
    display: flex;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    position: relative;
    background: #fff;
  }
  .menu li {
    flex:1;

    font-size: 0.28rem;
    line-height:0.28rem;
    padding: 0.26rem 0;
    text-align: center;
    color:rgb(77,85,93);
  }
 .router-link-exact-active.router-link-active{
    color:rgb(240,20,20);

  }
</style>
