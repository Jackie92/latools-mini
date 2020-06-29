<template>
<div class="gl">
  <div class="gl-head">
    <div class="fl">
      <div class="gl-head__title">绿化用地</div>
      <div class="gl-head__number">23<span class="m2">%</span> </div>
      <div class="gl-head__area-line">
        <div class="area-line__land-area">
          <p>1000㎡</p>
        </div>
      </div>
    </div>
    <div class="fr echarts-wrap">
      <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="demo-canvas" />

    </div>

  </div>
  <div class="gl-body">
    <div class="gl-body-head">
      <div class="three-switch">
        <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
        <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
        <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
      </div>
    </div>
    
    <div class="body-between">
      <div class="bet-item">
        <div class="bet-item-title">
          <div class="bet-item-left">
            <img class="greenarea" src="/static/icon/greenarea.png" alt="">绿化用地面积
          </div>
          <div class="bet-item-right">205860㎡</div>
        </div>
        <div class="green-pro-one">
          <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
          <div class="green-pro-left">79%</div>
        </div>
      </div>
      <div class="bet-item">
        <div class="bet-item-title">
          <div class="bet-item-left">
          <img class="greenarea" src="/static/icon/avegreen.png" alt="">人均绿地面积
          </div>
        </div>
        <div class="green-pro">
          <van-slider class="greenslider" bar-height="3.1px" active-color="#5380FF" inactive-color="#EBEBEB" value="50" @change="onChange" />
          <div class="green-pro-left">20㎡/人</div>
        </div>
      </div>
      <div class="bet-item">
        <div class="bet-item-title">
          <div class="bet-item-left">
            <img class="greenarea" src="/static/icon/avewater.png" alt="">人均水域面积
          </div>
        </div>
        <div class="green-pro">
          <van-slider class="greenslider" bar-height="3.1px" active-color="#5380FF" inactive-color="#EBEBEB" value="50" @change="onChange" />
          <div class="green-pro-left">180㎡/人</div>
        </div>
      </div>
    </div>
    <div class="body-foot">
      <div class="foot-item">
        <div class="foot-item-left"><img class="foot-img" src="/static/icon/tourist.png" alt="">游人容量</div>
        <div class="foot-item-right">5000人</div>
      </div>
      <div class="foot-item">
        <div class="foot-item-left"><img class="foot-img" src="/static/icon/charnum.png" alt="">座椅数量</div>
        <div class="foot-item-right">500-600个</div>
      </div>
      <div class="foot-item">
        <div class="foot-item-left"><img class="foot-img" src="/static/icon/wheelchair.png" alt="">放置轮椅</div>
        <div class="foot-item-right">500-600个</div>
      </div>
      <div class="foot-item-last">
        <div class="foot-last">
          <div class="foot-item-left"><img class="foot-img" src="/static/icon/latrinepit.png" alt="">蹲位数量</div>
          <div class="foot-item-right">500-600个</div>
        </div>
        <div class="foot-list-con">
          <div>女士蹲位数量</div>
          <div>500-600个</div>
        </div>
        <div class="foot-list-con">
          <div>男士蹲位数量</div>
          <div>500-600个</div>
        </div>
      </div>
    </div>
    <div class="submit">提交修改</div>
  </div>
</div>
  
</template>

<script>
import echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'

let chart = null

function initChart (canvas, width, height) {
  chart = echarts.init(canvas, null, {
    width: width,
    height: height
  })
  canvas.setChart(chart)

  var option = {
    color: ['#13987e', '#bbffd0', '#49e198'],
    series: [
      {
        type: 'pie',
        radius: ['90%', '70%'],
        avoidLabelOverlap: false,
        label: {
          show: false,
          position: 'center'
        },
        labelLine: {
          show: false
        },
        data: [
          {value: 1},
          {value: 3},
          {value: 2}
        ]
      }
    ]
  } // ECharts 配置项

  chart.setOption(option)

  return chart // 返回 chart 后可以自动绑定触摸操作
}
export default {
  components: {
    mpvueEcharts
  },
  data () {
    return {
      echarts,
      onInit: initChart,
      index: 0
    }
  },
  methods: {
    // sliderchange() {
    //   console.log(2333)
    // }
    onChange (event) {
      console.log(event)
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>
.gl {
  height: 100%;
}
.gl-head {
  display: block;
  background: -webkit-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -o-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -mos-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -moz-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: linear-gradient(left bottom,#08B26B,#0FC1A2);
  padding: 90px 20px 60px;
  color: #ffffff;
  overflow: hidden;
}
.gl-head__title {
  font-size: 22px;
}
.gl-head__number {
  font-size: 60px;
}
.gl-head__number .m2 {
  font-size: 22px;
  vertical-align: top;
}
.gl-head__area-line {
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
.gl-body {
  border-radius: 25px 25px 0 0;
  position: relative;
  top: -25px;
  background: #f7f7f7;
  box-sizing: border-box;
  padding-bottom: 20px;
}
.gl-body-head{
  position: relative;
  height: 56px;
  padding: 18px 20px;
  box-sizing: border-box;
}
.three-switch{
  width: 250px;
  height: 26px;
  border-radius: 13px;
  background: #ECECEC;
  float:right;
  display: flex;
  padding: 3px;
}
.switch-item{
  font-size: 15px;
  flex: 1;
  text-align: center;
  line-height: 26px;
  color: #A0A0A0;
}
.current{
  background: #ffffff;
  color: #11CC98;
  border-radius: 12px;
}
.body-between{
  padding: 10px 29px 5px;
}
.bet-item{
  margin-bottom: 24px;
}
.bet-item-title{
  font-size:15px;
  font-weight:600;
  color:#333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.green-pro{
  margin-top:10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.green-pro-one{
  margin-top:15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 18px;
}
.green-pro-left{
  font-size:15px;
  font-weight:600;
  line-height: 15px;
  color:#333333;
}
.greenpro{
  width: 210px;
}
.greenslider{
  width: 210px;
  margin-left: 18px;
}
.greenarea{
  width: 22px;
  height: 22px;
  background: #0FC0A3;
  border-radius: 11px;
  vertical-align: bottom;
  margin-right: 11px;
}
.body-foot{
  padding: 0px 15px 45px;
}
.foot-item{
  border-radius: 6px;
  background: #ffffff;
  padding: 20px;
  margin-bottom: 10px;
  font-size:15px;
  font-weight:600;
  color:#333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.foot-item-last{
  border-radius: 6px;
  background: #ffffff;
  padding: 20px;
  margin-bottom: 10px;
}
.foot-last{
  font-size:15px;
  font-weight:600;
  color:#333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.foot-list-con{
  font-size:15px;
  color:#333;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 13px;
}
.foot-img{
  width: 22px;
  height: 22px;
  background: #5983FE;
  border-radius: 11px;
  vertical-align: bottom;
  margin-right: 10px;
}
.submit{
  width: 315px;
  height: 54px;
  background: #12CC97;
  border-radius:27px;
  margin: auto;
  color: #ffffff;
  font-size:18px;
  font-weight:500;
  line-height:54px;
  text-align: center;
}
.chart-line {
  overflow: hidden;
}
.chart-line__block {
  float: left;
  width: 50%;
}
.chart-line__block__title {
  font-size: 20px;
}
.chart-edit {
  width: 30px;
  height: 30px;
  float: right;
}
.echarts-wrap {
  width: 50%;
  height: 150px;
}
</style>
