<template>
  <div class="entry-body">

    <!-- <div class="nav-title">景观小工具</div> -->
    <div class="entry-body__logo"><img src="/static/images/logo.png" alt=""></div>
    <div class="entry-body__start-btn white-bottom-btn" @click="goCalc">进入计算</div>
  </div>
</template>

<script>
export default {

  methods: {
    goCalc () {
      const url = '../parkType/main'
      mpvue.navigateTo({ url })
    }
  },

  created () {
    // let app = getApp()
    wx.clearStorageSync()
    wx.clearStorage()
  }
}
</script>

<style scoped>
.entry-body {
  display: block;
  height: 100%;
  width: 100%;
  background: -webkit-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -o-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -mos-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -moz-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: linear-gradient(left bottom,#08B26B,#0FC1A2);
}
.entry-body__logo {
  width: 100%;
  height: 40%;
  text-align: center;
  padding-top: 60%;
}
.entry-body__logo img {
  width: 120px;
  height: 120px;
  /* border-radius: 100%; */
}
.entry-body__start-btn {
  margin: 20% auto;
}
</style>
