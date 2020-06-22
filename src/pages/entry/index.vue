<template>
  <div class="entry-body">

    <!-- <div class="nav-title">景观小工具</div> -->
    <div class="entry-body__logo"><img src="/static/images/dlrb.jpg" alt=""></div>
    <div class="entry-body__start-btn" @click="goCalc">进入计算</div>
  </div>
</template>

<script>
export default {

  methods: {
    goCalc () {
      const url = '../logs/main'
      mpvue.navigateTo({ url })
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>
.entry-body {
  display: block;
  height: 100%;
  width: 100%;
  background: -webkit-linear-gradient(left bottom,#49E198,#13987E);
  background: -o-linear-gradient(left bottom,#49E198,#13987E);
  background: -mos-linear-gradient(left bottom,#49E198,#13987E);
  background: -moz-linear-gradient(left bottom,#49E198,#13987E);
  background: linear-gradient(left bottom,#49E198,#13987E);
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
  border-radius: 100%;
}
.entry-body__start-btn {
  margin: 20% auto;
  width: 90%;
  background: #FFFFFF;
  color: #13987E;
  text-align: center;
  border-radius: 25px;
  height: 40px;
  line-height: 40px;
  font-weight: 600;
}
</style>
