<!--
 * @author: QsPromise
 * @create: 2022-02-08 09:48 AM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-13 13:39 PM
 * @filePath: \min-app\src\pages\index\index.vue
 * @desc: 项目 min-app 首页文件
-->
<template>
  <view class="content">
    <view class="header" :style="{ marginTop: stateHeight, height: navHeight }">
      宏达万枫酒店
    </view>

    <view class="header-swiper">
      <u-swiper :list="headerSwiper"></u-swiper>
    </view>
    <view class="guess-you-like-box">
      <view class="fenge">
        <u-divider
          text="猜你喜欢"
          textColor="#23bdaa"
          lineColor="#23bdaa"
        ></u-divider>
      </view>
      <view>
        <swiper
          circular
          autoplay
          duration="1000"
          interval="1000"
          display-multiple-items="3"
          easing-function="linear"
        >
          <block v-for="(item, index) in roll" :key="index">
            <swiper-item>
              <view class="swiper-item-view" @click="descView">
                <image :src="item.img"></image>
                <view class="title">{{ item.title }}</view>
              </view>
            </swiper-item>
          </block>
        </swiper>
      </view>
    </view>
    <view class="tuijian">
      <view class="texts">今日推荐</view>
    </view>
    <block>
      <card @descView="descView">
        <template v-slot:btns
          ><button class="button" open-type="contact">客服</button></template
        >
      </card>
    </block>
    <login v-if="!isShowLogin"></login>
  </view>
</template>

<script>
import card from "@/components/card/card";
export default {
  components: {
    card,
  },
  data() {
    return {
      isShowLogin: false,
      navHeight: getApp().globalData.navHeight + "px",
      stateHeight: getApp().globalData.stateHeight + "px",
      title: "Hello",
      linkL: "https://www.baidu.com/",
      inWhiteList: false,
      headerSwiper: [
        "https://cdn.uviewui.com/uview/swiper/swiper1.png",
        "https://cdn.uviewui.com/uview/swiper/swiper2.png",
        "https://cdn.uviewui.com/uview/swiper/swiper3.png",
        "https://cdn.uviewui.com/uview/swiper/swiper1.png",
        "https://cdn.uviewui.com/uview/swiper/swiper2.png",
        "https://cdn.uviewui.com/uview/swiper/swiper3.png",
        "https://cdn.uviewui.com/uview/swiper/swiper1.png",
        "https://cdn.uviewui.com/uview/swiper/swiper2.png",
        "https://cdn.uviewui.com/uview/swiper/swiper3.png",
      ],
      roll: [
        {
          img: require("@/static/room/1.jpg"),
          title: "标间 ￥198.00",
        },
        {
          img: require("@/static/room/4.jpg"),
          title: "标间 ￥198.00",
        },
        {
          img: require("@/static/room/3.jpg"),
          title: "标间 ￥198.00",
        },
        {
          img: require("@/static/room/2.jpg"),
          title: "标间 ￥198.00",
        },
      ],
    };
  },
  onLoad() {},
  methods: {
    change() {
      // console.log("1111111111111");
      // console.log(uni.$u.config.v);
    },
    click() {
      console.log("222222222222222");
    },
    descView() {
      console.log("xiangqing");
    },
  },
};
</script>

<style lang="scss">
.content {
  background-color: #f0f0f0;
  padding-bottom: 1upx;
  min-height: 100vh;
  .header {
    color: #fff;
    font-size: 32upx;
    font-weight: bold;
    position: fixed;
    top: 0;
    width: 100vw;
    height: 100upx;
    line-height: 100upx;
    text-align: center;
    z-index: 100;
  }
  .header-swiper {
    position: sticky;
    top: 0;
    z-index: 99;
    width: 100vw;
  }
  .guess-you-like-box {
    margin: 20upx 0;
    background-color: #fff;
    .fenge {
      width: 400upx;
      margin: 0 auto;

      .u-divider {
        margin: 0 !important;
      }
    }
    swiper {
      height: 220upx !important;
    }
    .swiper-item-view {
      position: relative;
      height: 188upx;
      margin: 10upx;
      image {
        width: 100%;
        height: 100%;
      }
      .title {
        position: absolute;
        bottom: 0;
        width: 100%;
        text-align: center;
        font-size: 24upx;
        background-color: #ffffff80;
        border-radius: 10upx 10upx 0 0;
      }
    }
  }
  .tuijian {
    height: 60upx;
    line-height: 60upx;
    background-color: #fff;
    color: $uni-text-color-basic;
    font-size: 28upx;
    padding: 0 40upx;
    position: relative;
    .texts {
      padding-left: 20upx;
      &::before {
        content: "";
        height: 40upx;
        background-color: $uni-text-color-basic;
        width: 10upx;
        position: absolute;
        top: 50%;
        transform: translate(-200%, -50%);
      }
    }
  }
  .button {
    width: 100%;
    height: 100%;
    padding: 0;
    border: none;
    outline: none;
    background-color: transparent;
    line-height: 50upx;
    font-size: 24upx;
    text-align: center;
    border-radius: 10upx;
    color: #fff;
  }
}
</style>
