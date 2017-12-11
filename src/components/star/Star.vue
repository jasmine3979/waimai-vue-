<template>
    <div :class="star">
        <span class="star-item" v-for="item in itemClass" :class="item"></span>
    </div>
</template>

<script>
  const LENGTH=5;
  export default{
    props:['score','width'],
    data(){
        return {
            star:''
        }
    },
    created(){
//          console.log(this.score);
     this.star="star-"+this.width;


    },
    computed:{
        itemClass(){
            let stars=[];
            let zheng=parseInt(this.score);
            let xiao=this.score-zheng;
            for(let i=0;i<zheng;i++){
               stars.push('on');
            };
            if(xiao>=0.5){
                stars.push('half');
            };
            while(stars.length<LENGTH){
                stars.push('off');
            };
          return  stars;
        }

    }
  }
</script>

<style>
   .star-48 .star-item{
      display: inline-block;
      width:0.48rem;
      height:0.48rem;
      margin-right:0.48rem;
      background-size: 0.48rem 0.48rem;
      background-repeat: no-repeat;
    }

   .star-36 .star-item{
     display: inline-block;
     width:0.36rem;
     height:0.36rem;
     margin-right:0.16rem;
     background-size: 0.36rem 0.36rem;
     background-repeat: no-repeat;
   }


   .star-24 .star-item{
     display: inline-block;
     width:0.24rem;
     height:0.24rem;
     margin-right:0.1rem;
     background-size: 0.24rem 0.24rem;
     background-repeat: no-repeat;
   }
    .star-item.on{
      background-image: url('images/star48_on@2x.png');
    }
   .star-item.half{
        background-image: url('images/star48_half@2x.png');
    }
   .star-item.off{
      background-image: url('images/star48_off@2x.png');
    }

</style>
