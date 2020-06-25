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
        <div class="chart-line__block__title"><span>绿化用地(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>60</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="chart1" />
          </div>
        </div>
        <div><span>340000</span>㎡</div>
      </div>
      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>绿化用地(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>60</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="chart2" />
          </div>
        </div>
        <div><span>340000</span>㎡</div>
      </div>
      
    </div>
    <div class="chart-line">

      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>绿化用地(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>60</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="chart3" />
          </div>
        </div>
        <div><span>340000</span>㎡</div>
      </div>
      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>绿化用地(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>60</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="chart4" />
          </div>
        </div>
        <div><span>340000</span>㎡</div>
      </div>
      
    </div>
    
    <div class="ov__tab-bar">
      <div class="ov__tab-block ov__tab-block--left" :class="{'tab-block--active': leftActive}" @click="leftActive = true">人均绿地指标范围</div>
      <div class="ov__tab-block ov__tab-block--right" :class="{'tab-block--active': !leftActive}" @click="leftActive = false">建筑和数量</div>
    </div>

    <div class="ov__body">

      <div class="ov__body__line">
        <div class="fl">
          <img src="/static/images/people.png" alt="" class="body-line__icon">
          <span>&nbsp;&nbsp;游人容量</span>
          <span>&nbsp;&nbsp;&nbsp;&nbsp;2989人</span>
        </div>
        <div class="fr">
          <div class=" fl">|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>
          <div class="ov-body-line__area fr">
            <p>2000人</p>
            <p>-</p>
            <p>2000人</p>
          </div>
        </div>
      </div>
      <div class="ov__body__line ov__body__line--end">
        <div class="fl">
          <img src="/static/images/people.png" alt="" class="body-line__icon">
          <span>&nbsp;&nbsp;游人容量</span>
          <span>&nbsp;&nbsp;&nbsp;&nbsp;2989人</span>
        </div>
        <div class="fr">
          <div class=" fl">|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>
          <div class="ov-body-line__area fr">
            <p>2000人</p>
            <p>-</p>
            <p>2000人</p>
          </div>
        </div>
      </div>

    </div>

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
    color: ['#cccccc', '#5480ff'],
    series: [
      {
        type: 'pie',
        hoverAnimation: false,
        radius: ['80%', '60%'],
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
          {value: 3}
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
      leftActive: true
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
.chart-line {
  overflow: hidden;
  width: 100%;
}
.chart-line__block {
  float: left;
  width: 44%;
  padding: 20px 10px;
}
.chart-line__block__title {
  font-size: 18px;
  font-weight: bold;
}
.chart-edit {
  width: 25px;
  height: 25px;
  float: right;
}
.chart-line__icon img{
  width: 20px;
  height: 20px;
  vertical-align: middle;
  margin-top: -10px;
}
.chart-line__icon {
  font-size: 46px;
  color: #5480ff;
  float: left;
  padding-top: 10px;
}
.echarts-wrap {
  width: 38%;
  height: 80px;
  float: right;
}
.ov__tab-bar {
  overflow: hidden;
  text-align: center;
  border-radius: 10px 10px 0 0;
}
.ov__tab-block {
  width: 50%;
  background: #08B26B;
  height: 40px;
  line-height: 40px;
  font-size: 15px;
  color: #005D49;
}
.ov__tab-block--left {
  float: left;
}
.ov__tab-block--right {
  float: right;
}
.tab-block--active {
  background: #19D0A8;
  color: #FFFFFF;
}
.ov__body__line {
  font-size: 16px;
  overflow: hidden;
  padding: 10px 20px;
  height: 40px;
  line-height: 40px;
  background: #FFFFFF;
}
.body-line__icon {
  width: 23px;
  height: 23px;
  position: relative;
  top: 5px;
}
.ov-body-line__area {
  line-height: 14px;
  text-align: center;
}
.ov__body__line--end {
  border-radius: 0 0 10px 10px;
}
</style>
