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
    <div class="three-switch">
      
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
      onInit: initChart
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
  padding: 20px;
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
