<!--
 * @author: QsPromise
 * @create: 2022-02-08 09:48 AM
 * @license: MIT
 * @QsPromise: QsPromise
 * @lastEditTime: 2022-03-20 14:06 PM
 * @filePath: \min-app\src\App.vue
 * @desc:
-->
<style lang="scss">
/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
@import "uview-ui/index.scss";
@import "@/static/style/iconfont.css";
</style>
<script>
export default {
  globalData: {
    getFormtTime(time = +new Date(), type, str) {
      var date = new Date(time + 8 * 3600 * 1000);
      return date.toJSON().substr(0, type).replace("T", " ").replace(/-/g, str);
    },
    requestGet(url, param, header) {
      return new Promise((resolve, reject) => {
        uni.request({
          url,
          method: "get",
          data: param,
          header,
          success(res) {
            resolve(res.data); //打印
          },
          fail(err) {
            reject(err);
          },
        });
      });
    },
    requestPost(url, data, header) {
      return new Promise((resolve, reject) => {
        uni.request({
          url,
          method: "post",
          data,
          header,
          success(res) {
            console.log("1111111111111");
            resolve(res); //打印
          },
          fail(err) {
            console.log("22222222222222222222");
            reject(err);
          },
        });
      });
    },
    navHeight: 0,
    stateHeight: 0,
  },
  onLaunch: function () {
    this.getNavHeight();
  },
  onShow: function () {
    console.log("App Show");
  },
  onHide: function () {
    console.log("App Hide");
  },
  methods: {
    //  获取状态栏信息
    getNavHeight() {
      let stateHeight = 0; //  接收状态栏高度
      let navHeight = uni.getMenuButtonBoundingClientRect().height; //  获取胶囊高度
      let top = 0;
      uni.getSystemInfo({
        success(res) {
          console.log("状态栏：" + res.statusBarHeight);
          stateHeight = res.statusBarHeight;
        },
      });
      top = uni.getMenuButtonBoundingClientRect().top - stateHeight; //  获取top值
      //  然后将取到的值返回在data里面
      console.log(navHeight + top * 2, stateHeight);
      this.globalData.stateHeight = stateHeight;
      this.globalData.navHeight = navHeight + top * 2;
      console.log(this.globalData.stateHeight, this.globalData.navHeight);
    },
  },
};
</script>

<style>
/*每个页面公共css */
</style>
