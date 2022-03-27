/**$$
 $ @author: QsPromise
 $ @create: 2022-03-05 15:22 PM
 $ @license: MIT
 $ @LastEditor: QsPromise
 $ @lastEditTime: 2022-03-27 16:51 PM
 $ @filePath: \min-app\src\components\login\login.vue
 $ @desc:项目min-app 的组件 login 文件
 $*/
<template>
  <div class="login">
    <button open-type="getPhoneNumber" @getphonenumber="decryptPhoneNumber">
      登录
    </button>
  </div>
</template>

<script>
let app = getApp();
export default {
  name: "login",
  data() {
    return {};
  },
  methods: {
    decryptPhoneNumber(e) {
      console.log("logon", e);
      let objCode = e.detail.encryptedData,
        objIv = e.detail.iv;
      uni.login({
        success(res) {
          console.log("lohin", res);
        },
      });
    },
  },
};
</script>

<style lang='scss' scoped>
</style>