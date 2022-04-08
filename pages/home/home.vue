<template>
  <view>
    <view>
      <!-- 轮播图区域 -->
      <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
        <!-- 循环渲染轮播图的 item 项 -->
        <swiper-item v-for="(item, i) in swiperList" :key="i">
          <view class="swiper-item">
            <!-- 动态绑定图片的 src 属性 -->
            <image :src="item.imageSrc"></image>
          </view>
        </swiper-item>
      </swiper>
    </view>

    <!-- 分类导航区域 -->
    <view class="nav-list">
      <view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navClickHandler(item)">
        <image :src="'../../static/category_navi/'+item.imageSrc" class="nav-img"></image>
      </view>
    </view>

    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 楼层 item 项 -->
      <view class="floor-item" v-for="(item, i) in floorList" :key="i">
        <!-- 楼层标题 -->
        <view class="floor-title">{{item.floorName}}</view>
      </view>
    </view>

    <view>
      <view v-for="(item1,i1) in floorList[0].data" :key="i1" class="floor-box">
        <view>
          <image src="../../static/teacher/at.jpg" class="floor-info-image"></image>
          <text>{{item1.name}}</text>
        </view>
      </view>

    </view>

  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 1. 轮播图的数据列表，默认为空数组
        swiperList: [],
        // 1. 分类导航的数据列表
        navList: [],
        // 1. 楼层的数据列表
        floorList: [],
      }
    },
    onLoad() {
      // 2. 在小程序页面刚加载的时候，调用获取轮播图数据的方法
      this.getSwiperList()
      // 2. 在 onLoad 中调用获取数据的方法
      this.getNavList()
      // 2. 在 onLoad 中调用获取楼层数据的方法
      this.getFloorList()
    },
    methods: {
      // 3. 获取轮播图数据的方法
      async getSwiperList() {
        // 3.1 发起请求
        const {
          data: res
        } = await uni.$http.get('/getSwiper')
        // 3.2 请求失败
        if (res.status !== 200) {
          return uni.$showMsg()
        }
        // 3.3 请求成功，为 data 中的数据赋值
        this.swiperList = res.list
      },
      // 3. 获取轮播图数据的方法
      async getNavList() {
        // 3.1 发起请求
        const {
          data: res
        } = await uni.$http.get('/getCategoryNavi')
        // 3.2 请求失败
        if (res.status !== 200) {
          return uni.$showMsg()
        }
        // 3.3 请求成功，为 data 中的数据赋值
        this.navList = res.data
      },
      async getFloorList() {
        const {
          data: res
        } = await uni.$http.get('/getFloorData')
        if (res.status !== 200) return uni.$showMsg()
        this.floorList = res.data
        console.log(this.floorList)
      },

      // nav-item 项被点击时候的事件处理函数
      navClickHandler(item) {
        // 判断点击的是哪个 nav
        if (item.name === '分类') {
          uni.switchTab({
            url: '/pages/cate/cate'
          })
        }
      },
    },
  }
</script>

<style lang="scss">
  swiper {
    height: 330rpx;

    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }

  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;

    .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  }

  .floor-title {
    height: 60rpx;
    width: 100%;
    display: flex;
  }

  .floor-box {
    display: flex;
    padding: 20rpx;
    border: 2px solid #18BC37 ;
  }

  .floor-info-image {
    height: 160rpx;
    width: 160rpx;
  }
</style>
