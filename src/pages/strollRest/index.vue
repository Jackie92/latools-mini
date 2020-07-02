<template>
<div class="gl">
  <div class="gl-head">
    <div class="fl">
      <div class="gl-head__title">游憩服务用地</div>
      <div class="gl-head__number">80<span class="m2">%</span> </div>
      <div class="gl-head__area-line">
        <div class="area-line__land-area">
          <p>5860㎡</p>
        </div>
      </div>
    </div>
    <div class="fr echarts-wrap">
      <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="demo-canvas" />

    </div>

  </div>
  <div class="gl-body">
    <div class="gl-body-type">
      <div class="type-item"  @click="typeIn=0" :class="typeIn==0 ? 'active':''">游憩类建筑</div>
      <div class="type-item" @click="typeIn=1" :class="typeIn==1 ? 'active':''">服务类建筑</div>
    </div>
    <div class="swiper area-swiper">
      <swiper
        class="swiper-inner"
        :indicator-dots='false'
        :autoplay='false'
        :interval='3000'
        :circular='circular'
        :previous-margin="'100px'"
        :next-margin="'150px'"
        @change='changeSwiper'
        v-if="showSwiper"
      >
        <a  v-for='img in topSwipers' :key='img.id'>

          <swiper-item class="swiper-bar" :item-id='img.id'>
            <img              
              class='slide-image' 
              mode='aspectFit' 
              :src="img.outterImage" 
              />
          </swiper-item>
        </a>
      </swiper>  
      
    </div>
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
            <img class="greenarea" src="/static/icon/play.png" alt="">活动馆用地
          </div>
          <div class="bet-item-right">2个</div>
        </div>
        <div class="bet-item-be">500㎡</div>
        <div class="green-pro-one">
          <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
          <div class="green-pro-left">79%</div>
        </div>
      </div>
      <div class="bet-item">
        <div class="bet-item-title">
          <div class="bet-item-left">
            <div class="number">1</div>活动馆1
          </div>
        </div>
        <div class="green-pro">
          <van-slider class="greenslider" bar-height="3.1px" active-color="#5380FF" inactive-color="#EBEBEB" value="50" @change="onChange" />
          <div class="green-pro-left">15㎡/人</div>
        </div>
      </div>
      <div class="bet-item">
        <div class="bet-item-title">
          <div class="bet-item-left">
            <div class="number">2</div>活动馆2
          </div>
        </div>
        <div class="green-pro">
          <van-slider class="greenslider" bar-height="3.1px" active-color="#5380FF" inactive-color="#EBEBEB" value="50" @change="onChange" />
          <div class="green-pro-left">20㎡/人</div>
        </div>
      </div>
      <div class="add"><div class="number">+</div>添加类目</div>
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
      index: 0,
      typeIn: 0,
      showSwiper: true,
      topSwipers: [{
        id: 'huodong',
        outterImage: '../../static/images/dlrb.jpg'
      },
      {
        id: 'zhanlan',
        outterImage: '../../static/images/dlrb.jpg'
      },
      {
        id: 'tinglang',
        outterImage: '../../static/images/dlrb.jpg'
      }]
    }
  },
  methods: {
    changeSwiper (event) {
      console.log(event);
      
    }
    // sliderchange() {
    //   console.log(2333)
    // }
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
.gl-body-type{
  padding: 20px 20px 0px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.type-item{
  width: 90px;
  height: 30px;
  font-size:17px;
  font-weight:500;
  color:#666666;
  text-align: center;
  border-bottom: 2px solid #f7f7f7;
}
.active{
  color:#0EBE98;
  border-bottom: 2px solid #0EBE98;
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
.bet-item-be{
  font-size:15px;
  font-weight:600;
  color:#333;
  margin: 5px 0px 0px 33px;
}
.bet-item-left{
  display: flex;
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
}
.greenarea{
  width: 22px;
  height: 22px;
  background: #5480FF;
  border-radius: 11px;
  vertical-align: bottom;
  margin-right: 11px;
}
.add{
  font-size:15px;
  font-weight:600;
  color:#333;
  margin: 0px 0px 38px;
  display: flex;
}
.number{
  width: 22px;
  height: 22px;
  background: #5480FF;
  border-radius: 11px;
  vertical-align: bottom;
  margin-right: 11px;
  color: #ffffff;
  font-size:15px;
  font-weight:400;
  line-height:21px;
  text-align: center;
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
.area-swiper {
  height: 60px;
  margin-top: 20px;
}
.swiper-inner {
  height: 100%;
}
.swiper-bar {
  text-align: center;
}
.slide-image{
  width:134px;
  height:60px;
  margin: 0 10px;
}
</style>
