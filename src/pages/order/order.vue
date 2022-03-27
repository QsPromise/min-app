<!--
 * @author: QsPromise
 * @create: 2022-03-05 15:22 PM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-13 16:35 PM
 * @filePath: \min-app\src\pages\order\index.vue
 * @desc:项目min-app 的订单页面文件
-->
<template>
  <div class="order">
    <view class="list-tab">
      <u-tabs :list="orderList" @click="changeOrderType()"></u-tabs>
    </view>
    <view class="list-box">
      <swiper :current="current">
        <swiper-item>
          <view class="list all">
            <block>
              <view class="list-item">
                <view class="row row1">
                  <view>名称</view>
                  <view>状态</view>
                </view>
                <view class="row row2">
                  <view class="row">
                    <view class="img">图片</view>
                    <view>
                      <view>名称</view>
                      <view>金额</view>
                    </view>
                  </view>
                  <view>数量</view>
                </view>
                <view class="row row3">
                  <view>时间</view>
                  <view>金额</view>
                </view>
                <view class="row row4">
                  <view></view>
                  <view>再来一单</view>
                </view>
              </view>
            </block>
          </view>
        </swiper-item>
        <swiper-item>
          <view class="list obligation">2222</view>
        </swiper-item>
        <swiper-item>
          <view class="list receiving">4444</view>
        </swiper-item>
        <swiper-item>
          <view class="list stocks">555555555</view>
        </swiper-item>
        <swiper-item>
          <view class="list canceled">6666666666666</view>
        </swiper-item>
      </swiper>
    </view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: 0,
      orderList: [
        { name: "全部" },
        { name: "待付款" },
        { name: "待收货" },
        { name: "已完成" },
        { name: "已取消" },
      ],
    };
  },
  methods: {
    changeOrderType(e) {
      console.log(e.index);
      let index = e.index;
      this.current = e.index;
    },
  },
};
</script>

<style lang='scss' scoped>
.order {
  background-color: #f0f0f0;
  min-height: 100vh;
  .list-tab {
    display: flex;
    justify-content: center;
    background-color: #fff;
    box-shadow: 0 0 10upx #00000040;
  }
  .list-box {
    height: calc(100vh - 88upx);
    .list-item {
      margin: 20upx;
      padding: 10upx;
      border-radius: 10upx;
      box-shadow: 0 0 8upx #00000040;
      position: relative;
      background-color: #fff;
    }
    .row {
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      justify-content: space-between;
      align-items: center;
      .img {
        margin-right: 10upx;
      }
    }
  }
}
</style>