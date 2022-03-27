<!--
 * @author: QsPromise
 * @create: 2022-03-05 15:22 PM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-27 13:43 PM
 * @filePath: \min-app\src\pages\my\my.vue
 * @desc:项目min-app 的个人中心页面文件
-->
<template>
  <view class="my">
    <view class="header" :style="{ marginTop: stateHeight, height: navHeight }">
      宏达万枫酒店
    </view>
    <view class="top-box" :style="{ paddingTop: topHeight }">
      <view class="card">
        <view class="user">
          <view class="users">
            <view>
              <u-avatar :src="avatarImg"></u-avatar>
            </view>
            <view>
              <view>姓名</view>
              <view class="phone">13888888888</view>
            </view>
          </view>
          <view class="btn" @click="pwdView">
            <u-button type="success" size="small" text="查看密码"></u-button>
          </view>
        </view>
        <view class="bottoms">
          <view class="botton-left">
            <view class="user">您已入住万枫酒店1天</view>
            <view class="user">您的房间是:888</view>
          </view>
          <view class="btn" @click="pwdView">
            <u-button type="error" size="small" text="退房"></u-button>
          </view>
        </view>
      </view>
    </view>
    <view class="list">
      <u-cell-group>
        <u-cell icon="edit-pen-fill" title="评价" isLink></u-cell>
        <u-cell icon="setting-fill" title="设置" isLink></u-cell>
      </u-cell-group>
    </view>
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
export default {
  data() {
    return {
      navHeight: getApp().globalData.navHeight + "px",
      stateHeight: getApp().globalData.stateHeight + "px",
      topHeight:
        getApp().globalData.navHeight + getApp().globalData.stateHeight + "px",
      showCam: false,
      avatarImg: "https://cdn.uviewui.com/uview/album/1.jpg",
    };
  },
  methods: {
    pwdView() {
      this.showCam = true;
    },
    errorCam(e) {
      console.log("未授权", e);
      uni.showToast({
        title: "未授权",
        icon: "none",
      });
    },
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
                  url = "https://api-cn.faceplusplus.com/facepp/v3/compare",
                  data = {
                    api_key: "5fCeJCqXi-M0vdPpbPtStPxl7S1x6jJM", //请填写你创建的api_key
                    api_secret: "868RXYrQhK5R_3L2ts8YUC9uNP-8-q72", //请填写你的api_secret
                    face_token1: "0f8176e251262104900c824111f4323e", //传入face_token和脸集中的数据比对  后端返回在房间信息中
                    face_token2: face_token,
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
                    if (res.data.confidence < 80) {
                      wx.showModal({
                        title: "提示",
                        content: "面部不匹配",
                        showCancel: false,
                      });
                    } else {
                      //faqiqingqiu
                      wx.showModal({
                        title: "您的密码是",
                        content: "190897",
                        showCancel: false,
                      });
                      null;
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
  },
};
</script>

<style lang='scss' scoped>
.my {
  background-color: #f0f0f0;
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
  .top-box {
    background-color: #353a51;
    height: 350upx;
    padding: 1upx;
    .card {
      margin: 40upx;
      padding: 40upx;
      background-image: linear-gradient(to right, #7c8598, #b7c7d1);
      border-radius: 10upx;
    }
    .user,
    .users {
      display: flex;
      align-items: center;
      flex-direction: row;
      flex-wrap: nowrap;
      align-content: center;
      justify-content: space-between;
    }
    .users {
      width: 300upx;
      .phone {
        color: #333;
        font-size: 26upx;
      }
    }
    .bottoms {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
  }
  .list {
    margin-top: 20upx;
    background-color: #fff;
  }
}
</style>