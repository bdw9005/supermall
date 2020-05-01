<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav"><div slot="center">首页</div></nav-bar>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">

      <home-swiper :banners="advertises"/>
      <recommend-view :recommends="brands"/>
      <feature-view :features="newProducts"/>
      <tab-control class="tab-control"
                   :titles="['热门', '新款']"
                   @tabClick="tabClick"/>
      <good-list :goods="showProducts"/>
    </scroll>
    <div>呵呵呵呵</div>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'

  import { getHomeContentData,getHomeProductsData } from "network/home"

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodList,
      Scroll,
      BackTop
    },
    data() {
      return {
        advertises: [],
        brands: [],
        hotProducts: [],
        newProducts: [],
        subjects: [],
        homeFlashPromotion: [],
        products: {
          'pop': {pageNum: 0, list: []},
          'new': {pageNum: 0, list: []},
          // 'sell': {page: 0, list: []},
        },

        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          // 'sell': {page: 0, list: []},
        },
        currentType: 'pop',
        isShowBackTop: false
      }
    },
    computed: {
      showProducts() {
        return this.products[this.currentType].list
      }
    },
    created() {
      // 1.请求多个数据
      this.getHomeContentData()
      // 2.请求商品数据
      this.getHomeProductsData('pop')
      this.getHomeProductsData('new')
      // this.getHomeGoods('sell')
    },
    methods: {
      /**
       * 事件监听相关的方法
       */
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          // case 2:
          //   this.currentType = 'sell'
          //   break
        }
      },
      backClick() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      contentScroll(position) {
        this.isShowBackTop = (-position.y) > 1000
      },
      loadMore() {
        // this.getHomeGoods(this.currentType)
        this.getHomeProductsData(this.currentType)
      },
      /**
       * 网络请求相关的方法
       * 轮播图等内容
       */
      getHomeContentData() {
        getHomeContentData().then(res => {
          // console.log(res)
          this.advertises = res.data.advertiseList;
          this.brands =res.data.brandList;
          this.hotProducts =res.data.hotProductList;
          this.newProducts =res.data.newProductList;
          this.subjects =res.data.subjectList;
          this.homeFlashPromotion =res.data.homeFlashPromotion;
        })
      },
      getHomeProductsData(type) {
        const pageNum = this.products[type].pageNum + 1
        const pageSize = 4
        getHomeProductsData(type, pageNum,pageSize).then(res => {
          // console.log("---1---")
          // console.log(res)
          this.products[type].list.push(...res.data)
          this.products[type].pageNum += 1
          this.$refs.scroll.finishPullUp()
          // console.log("----2--")
          // console.log(this.products[type].list)
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }

  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  /*.content {*/
    /*height: calc(100% - 93px);*/
    /*overflow: hidden;*/
    /*margin-top: 44px;*/
  /*}*/
</style>
