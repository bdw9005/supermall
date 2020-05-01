<template>
  <div class="feature">
    <!--<a href="https://act.mogujie.com/zzlx67">-->
      <!--<img src="~assets/img/home/recommend_bg.jpg" alt="">-->
    <!--</a>-->
    <div v-for="item in features" class="feature-item">
      <a :href="item.bigPic">
        <img :src="item.pic" alt="">
        <div>{{item.name}}</div>
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: "FeatureView",
    props:{
      features: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style scoped>
  /*.feature img {*/
    /*width: 100%;*/
  /*}*/

  .feature {
    display: flex;
    width: 100%;
    text-align: center;
    font-size: 12px;

    padding: 10px 0 20px;
    border-bottom: 10px solid #eee;
  }

  .feature-item {
    flex: 1;
  }

  .feature-item img {
    width: 70px;
    height: 70px;
    margin-bottom: 10px;
  }
</style>
