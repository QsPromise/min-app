<!--
 * @author: QsPromise
 * @create: 2022-03-05 14:52 PM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-06 15:14 PM
 * @filePath: \min-app\src\components\card\card.vue
 * @desc:项目min-app 的组件 card 文件
-->
<template>
  <view class="card">
    <view class="left" @click="descView">
      <image src="@/static/room/1.jpg"></image>
    </view>
    <view class="right" @click="descView">
      <view class="names">房间名称</view>
      <view class="tags">
        <view class="u-tags">
          <u-tag text="标签" type="warning" size="mini" plain plainFill></u-tag>
        </view>
        <view class="u-tags">
          <u-tag text="标签" type="warning" size="mini" plain plainFill></u-tag>
        </view>
        <view class="u-tags">
          <u-tag text="标签" type="warning" size="mini" plain plainFill></u-tag>
        </view>
      </view>
      <view class="price">￥198</view>
      <view class="score">
        <u-rate
          active-color="#FA3534"
          inactive-color="#b2b2b2"
          gutter="2"
          :value="3.5"
          allowHalf
        ></u-rate>
        <view>推荐</view>
      </view>
      <view class="btn" @click.stop="btnClick">
        <slot name="btns"></slot>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: "card",
  data() {
    return {};
  },
  methods: {
    btnClick() {
      this.$emit("btnClick");
    },
    descView() {
      this.$emit("descView");
    },
  },
};
</script>

<style lang='scss' scoped>
.card {
  display: flex;
  gap: 30upx;
  background-color: #fff;
  margin: 20upx;
  padding: 10upx;
  border-radius: 10upx;
  box-shadow: 0 0 8upx #00000040;
  position: relative;
  .left {
    flex-shrink: 0;
    width: 260upx;
    height: 200upx;
    /* #ifdef MP-WEIXIN */
    margin-right: 20upx;
    /* #endif  */
    image {
      width: 100%;
      height: 100%;
    }
  }
  .right {
    flex: 1;
    .tags {
      display: flex;
      gap: 10upx;
      padding: 5upx 0;
      box-sizing: border-box;
      /* #ifdef MP-WEIXIN */
      .u-tags {
        margin-right: 20upx !important;
      }
      /* #endif  */
    }
    .score {
      display: flex;
      gap: 20upx;
      font-size: 28upx;
      align-items: center;
      position: relative;
      &::after {
        content: "";
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
      }
    }
    .btn {
      position: absolute;
      bottom: 0;
      right: 0;
      transform: translate(-20upx, -20upx);
      width: 70upx;
      height: 50upx;
      line-height: 50upx;
      background-color: #4abdcc;
      font-size: 24upx;
      text-align: center;
      border-radius: 10upx;
      color: #fff;
    }
  }
}
</style>