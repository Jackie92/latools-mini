<template>
<div class="overview">
  <div class="overview-head">
    <div class="overview-head__title">场地总面积</div>
    <div class="overview-head__number">2333 <span class="m2">㎡</span> </div>
    <div class="overview-head__area-line">
      <div class="area-line__land-area">
        <p>1</p>
        <p>陆地面积(㎡)</p>
      </div>
      <div class="area-line__water-area">
        <p>1</p>
        <p>水域面积(㎡)</p>
      </div>
    </div>
  </div>
  <div class="overview-body">
    <div class="chart-line">
      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>绿化用地</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
      </div>
    </div>
  </div>
</div>
  
</template>

<script>
export default {
  data () {
    return {
    }
  },
  methods: {
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>
.overview {
  height: 100%;
}
.overview-head {
  display: block;
  background: -webkit-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -o-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -mos-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -moz-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: linear-gradient(left bottom,#08B26B,#0FC1A2);
  padding: 90px 20px 60px;
  color: #ffffff;
}
.overview-head__title {
  font-size: 22px;
}
.overview-head__number {
  font-size: 40px;
}
.overview-head__number .m2 {
  font-size: 22px;
  vertical-align: top;
}
.overview-head__area-line {
  overflow: hidden;
  font-size: 18px;
  padding-top: 10px;
}
.area-line__land-area {
  float: left;
}
.area-line__water-area {
  float: right;
  text-align: right;
}
.overview-body {
  border-radius: 25px 25px 0 0;
  position: relative;
  top: -25px;
  background: #f7f7f7;
  padding: 20px;
}
</style>
