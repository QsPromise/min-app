<!--
 * @author: QsPromise
 * @create: 2022-03-05 11:12 AM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-27 14:57 PM
 * @filePath: \min-app\src\pages\predetermine\predetermine.vue
 * @desc:项目 min-app 预定页文件
-->
<template>
  <view class="predetermine">
    <view class="header-swiper">
      <u-swiper :list="headerSwiper"></u-swiper>
    </view>
    <view class="merchant">
      <view class="names">宏达万枫酒店</view>
      <view class="share my-icon minapp-fenxiang">
        <button open-type="share"></button>
      </view>
      <view class="merchant-bottom">
        <view @click="locationView">
          <text class="mb-text my-icon minapp-daohang-yimidida-"></text>
          <text>一键导航</text>
        </view>
        <u-line direction="col" color="#23bdaa"></u-line>
        <view @click="phones">
          <text class="mb-text my-icon minapp-dianhua-yimidida-"></text>
          <text>联系电话</text>
        </view>
      </view>
    </view>
    <view class="list">
      <u-tabs
        scrollable
        lineWidth="60"
        lineColor="#23bdaa"
        :list="preClass"
        @click="tabsClick"
        :activeStyle="{
          color: '#303133',
          fontWeight: 'bold',
          transform: 'scale(1.05)',
        }"
        :inactiveStyle="{
          color: '#606266',
          transform: 'scale(1)',
        }"
        itemStyle="width: 50vw; height: 34px; text-align: center;padding: 0"
      ></u-tabs>
    </view>
    <view class="slide-box">
      <view class="slide-left" :style="{ left: slideLeft }">
        <view class="romms">
          <view class="ruzhu" @click="selSta">
            <view>入住时间</view>
            <view class="days">{{ staTime }}</view>
          </view>
          <view class="times">{{ hotelTimes }}</view>
          <view class="lidian" @click="selEnd">
            <view>离店时间</view>
            <view class="days">{{ endTime }}</view>
          </view>
        </view>
        <block>
          <card @btnClick="preRooom" @descView="descView(0)">
            <template v-slot:btns>预定</template>
          </card>
        </block>
      </view>
      <view class="slide-right" :style="{ left: slideRight }">
        <view class="right-top">
          <view class="class-list">
            <block v-for="(item, index) in dishesList" :key="index">
              <view
                class="class-item"
                :class="{ active: activeIndex == index }"
                @click="scrTo(item.class, index)"
              >
                {{ item.name }}
              </view>
            </block>
          </view>
          <view class="dishes-list">
            <scroll-view
              class="scroll-dishes"
              id="demo"
              scroll-y
              :style="'height:calc(100vh - 100upx - ' + scrollTop + 'px );'"
              @scroll="scrollDishes"
              :scroll-into-view="scrView"
            >
              <block v-for="(item, index) in dishesList" :key="index">
                <view class="dishes-item" :id="item.class">
                  <view>{{ item.name }}</view>
                  <block v-for="(itm, idx) in item.childs" :key="idx">
                    <view class="item-box">
                      <view class="img">
                        <image :src="itm.imgUrl"></image>
                      </view>
                      <view>
                        <view class="item-right">{{ itm.names }}</view>
                        <view class="item-right">￥{{ itm.price }}</view>
                        <view class="item-right"
                          >- <input type="text" :value="1" /> +</view
                        >
                      </view>
                    </view>
                  </block>
                </view>
              </block>
            </scroll-view>
          </view>
        </view>
        <view class="shopCar">
          <view class="shop-price">￥100</view>
          <view class="shop-btn">结算</view>
        </view>
      </view>
    </view>
    <u-datetime-picker
      ref="datetimePicker"
      :show="datetimePickers"
      v-model="value1"
      mode="date"
      closeOnClickOverlay
      :formatter="formatter"
      @close="closePicker"
      @cancel="closePicker"
      @confirm="selPicker"
    ></u-datetime-picker>
    <u-popup :show="popshow" @close="closePop" :round="10" closeable>
      <view class="pop-item">
        <u--form labelPosition="left" :model="model1" labelWidth="90">
          <u-form-item
            label="姓名"
            prop="userInfo.name"
            borderBottom
            placeholder="请输入姓名"
          >
            <u--input v-model="model1.userInfo.name" border="none"></u--input>
          </u-form-item>
          <u-form-item label="身份证号码" prop="userInfo.name" borderBottom>
            <u--input
              v-model="model1.userInfo.IDnum"
              border="none"
              type="idcard"
              maxlength="18"
              @blur="sexSelect"
              placeholder="请输入身份证号码"
            ></u--input>
          </u-form-item>
          <u-form-item label="性别" prop="userInfo.sex" borderBottom>
            <u--input
              v-model="model1.userInfo.sex"
              disabled
              disabledColor="#ffffff"
              placeholder="自动获取性别"
              border="none"
            ></u--input>
          </u-form-item>
          <u-form-item label="住入时间" prop="userInfo.sex" borderBottom>
            <u--input
              v-model="staTime"
              disabled
              disabledColor="#ffffff"
              placeholder="请在上方选择"
              border="none"
            ></u--input>
          </u-form-item>
          <u-form-item label="离店时间" prop="userInfo.sex" borderBottom>
            <u--input
              v-model="endTime"
              disabled
              disabledColor="#ffffff"
              placeholder="请在上方选择"
              border="none"
            ></u--input>
          </u-form-item>
        </u--form>
        <view class="btn" @click="preRooms">
          <u-button type="primary" text="确定"></u-button>
        </view>
      </view>
    </u-popup>
    <camera
      v-if="showCam"
      device-position="front"
      flash="off"
      @error="errorCam"
      @initdone="initdoneCam"
      style="width: 100%; height: 300px"
    ></camera>
  </view>
</template>

<script>
let app = getApp();
export default {
  data() {
    return {
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
      preClass: [
        {
          name: "客房预订",
        },
        {
          name: "餐饮预定",
        },
      ],
      dishesList: [
        {
          name: "热菜",
          class: "rc",
          childs: [
            {
              imgUrl: require("../../static/cai/1.png"),
              names: "法式烤鸡",
              price: "18.80",
            },
          ],
        },
        {
          name: "凉菜",
          class: "lc",
        },
        {
          name: "西餐甜点",
          class: "xc",
        },
        {
          name: "酒水饮料",
          class: "js",
        },
      ],
      dishesIndex: 0,
      preIndex: 0,
      slideLeft: "0",
      slideRight: "100%",
      datetimePickers: false,
      dataPickerType: true,
      value1: new Date(new Date().toLocaleDateString()).getTime(),
      staTime: "",
      endTime: "",
      hotelTimes: "1晚",
      paramPre: {
        str: new Date(new Date().toLocaleDateString()).getTime(),
        end:
          new Date(new Date().toLocaleDateString()).getTime() +
          24 * 60 * 60 * 1000,
      },
      showCam: false,
      faceToken: "",
      scrollTop: 0,
      scrView: "rc",
      sceollChildTop: [],
      activeIndex: 0,
      popshow: false,
      model1: {
        userInfo: {
          name: "uView UI",
          sex: "",
          iDnum: "",
        },
      },
      actions: [
        {
          name: "男",
        },
        {
          name: "女",
        },
        {
          name: "保密",
        },
      ],
    };
  },
  onReady() {
    // 微信小程序需要用此写法
    this.$refs.datetimePicker.setFormatter(this.formatter);
    let times = Date.now();
    let times2 = Number(new Date());
    console.log(times2);
    this.staTime = app.globalData.getFormtTime(times, 10, "-");
    this.endTime = app.globalData.getFormtTime(
      times + 24 * 60 * 60 * 1000,
      10,
      "-"
    );
    this.getViewTop();
    /* 等待DOM挂载完成 */
    this.$nextTick(() => {
      /* 在非H5平台，nextTick回调后有概率获取到错误的元素高度，则添加200ms的延迟来减少BUG的产生 */
      setTimeout(() => {
        /* 等待滚动区域初始化完成 */
        this.qetScrollList();
      }, 200);
    });
  },
  methods: {
    share() {},
    locationView() {
      uni.openLocation({
        latitude: 29.501173,
        longitude: 102.646093,
        success: function () {
          console.log("success");
        },
      });
    },
    phones() {
      uni.makePhoneCall({
        phoneNumber: "18069896196", //仅为示例
      });
    },
    sectionChange(e) {
      console.log(e);
      this.dishesIndex = e;
      // 请求
    },
    closePop() {
      this.popshow = false;
    },
    tabsClick(item) {
      console.log("item", item);
      this.preIndex = item.index;
      if (item.index == 0) {
        this.slideLeft = "0";
        this.slideRight = "100%";
      } else {
        this.slideLeft = "-100%";
        this.slideRight = "0";
      }
    },
    sexSelect(e) {
      let str = this.model1.userInfo.IDnum;
      console.log(str.length);
      if (str.length != 18) {
        uni.showToast({
          icon: "none",
          title: "请输入合法的身份证号码",
        });
        return;
      }
      let sex = str.charAt(str.length - 2);
      if (sex % 2 == 1) {
        this.model1.userInfo.sex = "男";
      } else {
        this.model1.userInfo.sex = "女";
      }
    },
    descView(e) {
      console.log("xiangqing");
    },
    preRooms() {
      this.showCam = true;
      console.log("预定");
      this.popshow = false;
    },
    preRooom() {
      this.popshow = true;
    },
    preComestible(e) {
      console.log("canyin");
    },
    getViewTop() {
      let query = uni.createSelectorQuery().in(this);
      query
        .select("#demo")
        .boundingClientRect((data) => {
          console.log("元素距离顶部的距离" + data.top);
          this.scrollTop = data.top;
        })
        .exec();
    },
    qetScrollList() {
      let sceollChild = uni.createSelectorQuery().selectAll(".dishes-item");
      sceollChild
        .boundingClientRect((res) => {
          console.log(res);
          for (const item of res) {
            this.sceollChildTop.push(Math.round(item.top - this.scrollTop));
          }
          console.log(this.sceollChildTop);
        })
        .exec();
    },
    scrollDishes(e) {
      let num = e.detail.scrollTop;
      for (const item in this.sceollChildTop) {
        if (Math.abs(num - this.sceollChildTop[item]) < 5) {
          this.activeIndex = item;
          console.log(item);
        }
      }
    },
    // 滚动
    scrTo(e, n) {
      console.log(e);
      this.scrView = e;
      this.activeIndex = n;
    },
    //人脸检测
    initdoneCam(e) {
      let that = this;
      let ctx = uni.createCameraContext();
      ctx.takePhoto({
        quality: "high", //获取原图
        success: (res) => {
          let src = res.tempImagePath; //得到拍照后的图片地址
          uni.showToast({
            icon: "loading",
            title: "人脸检测中",
          });
          uni.uploadFile({
            url: "https://api-cn.faceplusplus.com/facepp/v3/detect",
            filePath: src, //刚才拍照的图片地址
            name: "image_file", //图片的字段名和接口的字段要对应上
            header: {
              "Content-Type": "multipart/form-data", //必须用此header
            },
            formData: {
              api_key: "5fCeJCqXi-M0vdPpbPtStPxl7S1x6jJM", //请填写你创建的api_key
              api_secret: "868RXYrQhK5R_3L2ts8YUC9uNP-8-q72", //请填写你的api_secret
            },
            success(res) {
              that.showCam = false;
              let obj = JSON.parse(res.data); //转换成json格式不然解析不了
              console.log(obj);
              if (obj["faces"][0] == null || obj["faces"][0] == "") {
                //根据反回的数据判断是是否检测到人脸
                wx.showModal({
                  title: "提示",
                  content: "检测不到人脸",
                  showCancel: true,
                });
                return;
              } else {
                let face_token = obj["faces"][0]["face_token"], //获取得到的人脸标识
                  url = "https://api-cn.faceplusplus.com/facepp/v3/search",
                  data = {
                    api_key: "5fCeJCqXi-M0vdPpbPtStPxl7S1x6jJM", //请填写你创建的api_key
                    api_secret: "868RXYrQhK5R_3L2ts8YUC9uNP-8-q72", //请填写你的api_secret
                    face_token: face_token, //传入face_token和脸集中的数据比对
                    outer_id: "QsPromiseOne", //脸集唯一标识，就是上面我们创建的脸集
                    return_result_count: "1", //返回一条匹配数据，范围1-5
                  },
                  header = {
                    "content-type": "application/x-www-form-urlencoded",
                  };
                uni.request({
                  url,
                  method: "post",
                  data: data,
                  header,
                  success(res) {
                    console.log(res.data, "ggggggggggggggggg"); //打印
                    that.faceToken = face_token;
                    console.log(that.faceToken);
                    if (res.data.error_message == "EMPTY_FACESET") {
                      that.addFacial(face_token);
                    } else {
                      let confidence = res.data["results"][0]["confidence"];
                      if (confidence < 80) {
                        that.addFacial(face_token);
                      } else {
                        null;
                      }
                    }
                  },
                  fail(err) {
                    console.log(err);
                  },
                });
              }
            },
          });
        },
        fail(err) {
          console.log(err);
        },
      });
      console.log(ctx);
      console.log(e, "LLLLLLLLLLLLLLLLLL");
    },
    // 将人脸加入人脸集
    addFacial(face_token) {
      wx.request({
        url: "https://api-cn.faceplusplus.com/facepp/v3/faceset/addface", //添加到脸集的接口
        method: "post",
        data: {
          api_key: "5fCeJCqXi-M0vdPpbPtStPxl7S1x6jJM", //请填写你创建的api_key
          api_secret: "868RXYrQhK5R_3L2ts8YUC9uNP-8-q72", //请填写你的api_secret
          face_tokens: face_token, //把请求得到的人脸标识添加到脸集中
          outer_id: "QsPromiseOne",
        },
        header: {
          "content-type": "application/x-www-form-urlencoded",
        },
        success(res) {
          console.log(res.data);
          wx.showModal({
            title: "提示",
            content: "验证过",
            showCancel: false,
          });
          let obj = { ...this.model1.userInfo, ...this.paramPre };
          //发起请求
          app.globalData.requestPost(that.ApiUrl.creatrOrder, {}, {});
        },
      });
      console.log("kongde");
    },
    // 相机初始化失败
    errorCam(e) {
      console.log("未授权", e);
      uni.showToast({
        title: "未授权",
        icon: "none",
      });
    },
    // 订餐购物车结算
    preRepast() {
      console.log("订餐");
    },
    // 选择入住日期
    selSta() {
      this.datetimePickers = true;
      this.dataPickerType = true;
      this.value1 = new Date(new Date().toLocaleDateString()).getTime();
    },
    //选择离店日期
    selEnd() {
      this.datetimePickers = true;
      this.dataPickerType = false;
      this.value1 =
        new Date(new Date().toLocaleDateString()).getTime() +
        24 * 60 * 60 * 1000;
    },
    closePicker() {
      this.datetimePickers = false;
    },
    selPicker(e) {
      if (this.dataPickerType) {
        console.log("入住时间", e);
        let t = app.globalData.getFormtTime(e.value, 10, "-");
        this.staTime = t;
        this.paramPre.str = e.value;
      } else {
        console.log("离店时间", e);
        let t = app.globalData.getFormtTime(e.value, 10, "-");
        this.endTime = t;
        this.paramPre.end = e.value;
        this.hotelTimes =
          Math.floor(
            (this.paramPre.end - this.paramPre.str) / (24 * 60 * 60 * 1000)
          ) + "晚";
        console.log(
          this.hotelTimes,
          this.paramPre.end,
          this.paramPre.str,
          (this.paramPre.end - this.paramPre.str) / (24 * 60 * 60 * 1000)
        );
      }
      this.datetimePickers = false;
    },
    formatter(type, value) {
      if (type === "year") {
        return `${value}年`;
      }
      if (type === "month") {
        return `${value}月`;
      }
      if (type === "day") {
        return `${value}日`;
      }
      return value;
    },
  },
};
</script>

<style lang='scss' scoped>
.predetermine {
  background-color: #f0f0f0;
  min-height: 100vh;
  .merchant {
    background-color: #fff;
    text-align: center;
    position: relative;
    .share {
      position: absolute;
      top: 0;
      right: 0;
      transform: translate(-100%, 100%);
      button {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        border: none;
        background: transparent;
        outline: none;
        padding: 0;
      }
    }
    .merchant-bottom {
      display: flex;
      height: 40upx;
      font-size: 26upx;
      gap: 20upx;
      flex-direction: row;
      justify-content: center;
      color: $uni-text-color-basic;
      .u-line {
        flex-shrink: 0;
      }
      /* #ifdef MP-WEIXIN */
      view:first-child {
        margin-right: 20upx;
      }
      view:last-child {
        margin-left: 20upx;
      }
      /* #endif  */
      .mb-text {
        margin-right: 20upx;
      }
    }
  }
  .list {
    background-color: #fff;
    margin-top: 20upx;
    padding-bottom: 1upx;
  }
  .slide-box {
    position: relative;
    > view {
      position: absolute;
      width: 100%;
      transition: left 0.5s;
    }
    .slide-right {
      .right-top {
        display: flex;
      }
      .class-list {
        width: 160upx;
        flex-shrink: 0;
        .class-item {
          text-align: justify;
          text-align-last: justify;
        }
        .active {
          background-color: #f0f;
        }
      }
      .scroll-dishes {
        height: 500upx;
        .item-box {
          display: flex;
          .img {
            width: 200upx;
            height: 200upx;
            overflow: hidden;
            image {
              width: 100%;
              height: 100%;
              display: block;
            }
          }
        }
      }
      .dishes-list {
        flex: 1;
      }
      .shopCar {
        display: flex;
        justify-content: space-between;
        box-sizing: border-box;
        padding: 0 40upx;
        height: 100upx;
        view {
          line-height: 100upx;
        }
        .shop-btn {
          background-color: #f0f;
        }
      }
    }
    .romms {
      display: flex;
      align-items: center;
      justify-content: space-around;
      text-align: center;
      background-color: #fff;
      border-top: 1px solid #efefef;
      font-size: 26upx;
      padding: 10upx 0;
      .times {
        position: relative;
        font-weight: bold;
      }
      .times::after {
        content: "";
        display: block;
        width: 180upx;
        height: 4upx;
        background-color: $uni-text-color-basic;
      }
      .times::before {
        content: "";
        position: absolute;
        bottom: 0;
        right: 0;
        width: 20upx;
        height: 4upx;
        transform: rotate(45deg);
        transform-origin: right;
        background-color: $uni-text-color-basic;
      }
      .days {
        color: $uni-text-color-basic;
      }
    }
  }
  .pop-item {
    padding: 60upx 20upx 10upx;
    .btn {
      margin: 10upx 0;
    }
  }
}
</style>