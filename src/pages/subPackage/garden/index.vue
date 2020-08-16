<template>
<div class="gl">
  <div class="gl-head">
    <div class="fl">
      <div class="gl-head__title">园路及铺装用地</div>
      <div class="gl-head__number">{{(allgardenSum * 100 / sdata.landArea )| numFilter}}<span class="m2">%</span> </div>
      <div class="gl-head__area-line">
        <div class="area-line__land-area">
          <p>{{allgardenSum}}㎡</p>
        </div>
      </div>
    </div>
    <div class="fr echarts-wrap">
      <mpvue-echarts :echarts="echarts" :onInit="onInit" canvasId="demo-canvas" />

    </div>

  </div>
  <div class="gl-body">
    <!-- 园路及铺装用地start -->
    <div class="gl-showBody" v-if="typeIn == 0">
      <div class="swiper area-swiper">
        <swiper
          class="swiper-inner"
          :indicator-dots='false'
          :autoplay='false'
          :interval='3000'
          :circular='circular'
          :previous-margin="'100px'"
          :next-margin="'100px'"
          :current='current'
          @change='changeSwiper'
        >
          <a  v-for='(img, index) in topSwipers' :key='index'>

            <swiper-item class="swiper-bar" :item-id='img.id'>
              <img              
                v-if="current === index"
                class='slide-image' 
                mode='aspectFit' 
                :src="img.outterImageCur" 
                />
              <img              
                v-else
                class='slide-image' 
                mode='aspectFit' 
                :src="img.outterImage" 
                />
            </swiper-item>
          </a>
        </swiper>  
        
      </div>

      <!-- 停车场start -->
      <div v-if="strollType === 'tingche'" class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serve[0] === 0">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serve[0] === 2">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index1=0" :class="index1==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index1=1" :class="index1==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">停车场用地
              </div>
              <div class="bet-item-right">{{parkingList.length}}个</div>
            </div>
            <div class="bet-item-be">{{parkingListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(parkingListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(parkingListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">停车位数量
              </div>
            </div>
            <div class="green-pro-one">
              <van-progress
                class="van-progress"
                :pivot-text="~~(parkingListAll / 30)"
                color="#5380FF"
                show-pivot
                :percentage="parkingListAll * 100 / (sdata.rateNum[3] / 100 * sdata.landArea) + 8"
              />
              <div class="green-pro-left">{{~~(parkingListAll / 30)}}个</div>
            </div>
          </div>
          <div v-if="parkingList.length > 0" v-for="(item, i) in parkingList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>停车场{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="0"
                  @change="onChangeList(i, 'tingche', $event)"
                  :min='0'
                  :max='lastSum'
                />
                <div class="green-pro-left">{{parkingList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add"  v-if="sdata.facility.serve[0] !== 0"><div class="number"  @click="addList('tingche')">+</div><div class="delete" @click="delList('tingche')">-</div></div>
          <div class="add"  v-else><div class="number" >+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 停车场end -->

      <!-- 自行车存放处start -->
      <div v-if="strollType === 'zixingche'" class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serve[1] === 0">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serve[1] === 2">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index2=0" :class="index2==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index2=1" :class="index2==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">自行车存放处
              </div>
              <div class="bet-item-right">{{bikeList.length}}个</div>
            </div>
            <div class="bet-item-be">{{bikeListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(bikeListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(bikeListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">停车位数量
              </div>
            </div>
            <div class="green-pro-one">
              <van-progress
                class="van-progress"
                :pivot-text="~~(bikeListAll / 1.7)"
                color="#5380FF"
                show-pivot
                :percentage="bikeListAll * 100 / (sdata.rateNum[3] / 100 * sdata.landArea) + 8"
              />
              <div class="green-pro-left">{{~~(bikeListAll / 1.7)}}个</div>
            </div>
          </div>
          <div v-if="bikeList.length > 0" v-for="(item, i) in bikeList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>自行车存放处{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="0"
                  @change="onChangeList(i, 'zixingche', $event)"
                  :min='0'
                  :max='lastSum'
                />
                <div class="green-pro-left">{{bikeList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add"  v-if="sdata.facility.serve[1] !== 0"><div class="number"  @click="addList('zixingche')">+</div><div class="delete" @click="delList('zixingche')">-</div></div>
          <div class="add"  v-else><div class="number" >+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 自行车存放处end -->

      <!-- 活动场地start -->
      <div v-if="strollType === 'huodong'" class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.recreation[3] === 0">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.recreation[3] === 2">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index3=0" :class="index3==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index3=1" :class="index3==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">活动场地
              </div>
              <div class="bet-item-right">{{activityList.length}}个</div>
            </div>
            <div class="bet-item-be">{{activityListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(activityListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(activityListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="activityList.length > 0" v-for="(item, i) in activityList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>活动场地{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="0"
                  @change="onChangeList(i, 'huodong', $event)"
                  :min='0'
                  :max='lastSum'
                />
                <div class="green-pro-left">{{activityList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add"  v-if="sdata.facility.recreation[3] !== 0"><div class="number"  @click="addList('huodong')">+</div><div class="delete" @click="delList('huodong')">-</div></div>
          <div class="add"  v-else><div class="number" >+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 活动场地end -->

      <!-- 园路start -->
      <div v-if="strollType === 'yuanlu'" class="swiper-per-body">
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">园路用地面积
              </div>
            </div>
            <div class="bet-item-be">{{pavement}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="pavementPercent" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{pavementPercent}}%</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 园路end -->

    </div>
    <!-- 园路及铺装用地end -->

    <div class="submit" @click="submit">提交修改</div>
  </div>
</div>
  
</template>

<script>
import echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'
import { mapState } from 'vuex'

let chart = null

function getOption (num) {
  const rate = wx.getStorageSync('area').rateNum
  const rateNum = rate[num]
  return {
    color: ['#187161', '#BBFFD0'],
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
          {value: 100 - ~~rateNum},
          {value: ~~rateNum}
        ]
      }
    ],
    index1: 0,
    index2: 0,
    index3: 0,
    index4: 0,
    allgardenSum: 0,
    lastSum: 0,
    gardenArea: 0
  }
}
export default {
  components: {
    mpvueEcharts
  },
  computed: mapState(['sdata']),
  data () {
    return {
      echarts,
      onInit: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(3))
        return chart
      },
      index: 0,
      typeIn: 0,
      topSwipers: [{
        id: 'tingche',
        outterImage: '../../../static/images/tcc.png',
        outterImageCur: '../../../static/images/tcc2.png'
      },
      {
        id: 'zixingche',
        outterImage: '../../../static/images/zxc.png',
        outterImageCur: '../../../static/images/zxc2.png'
      },
      {
        id: 'huodong',
        outterImage: '../../../static/images/hdcd.png',
        outterImageCur: '../../../static/images/hdcd2.png'
      },
      {
        id: 'yuanlu',
        outterImage: '../../../static/images/yuanlu.png',
        outterImageCur: '../../../static/images/yuanlu2.png'
      }],
      // sdata: {},
      greenPer: 0,
      peopleAbility: 0,
      strollType: 'tingche',
      parking1: 100,
      parking2: 200,
      parkingList: [],
      parkingListAll: 0,
      bike1: 150,
      bike2: 150,
      bikeList: [],
      bikeListAll: 0,
      activity1: 100,
      activity2: 200,
      activityList: [],
      activityListAll: 0,
      current: 0,
      pavement: 0,
      pavementPercent: 0,
      allgardenSum: 0,
      lastSum: 0,
      gardenArea: 0
    }
  },
  filters: {
    numFilter (value) {
      let realVal = ''
      if (!isNaN(value) && value !== '') {
        // 截取当前数据到小数点后两位
        realVal = parseFloat(value).toFixed(2)
      } else {
        realVal = '--'
      }
      return realVal
    }
  },
  methods: {
    submit () {
      this.sdata.parkNum = this.parkingList.length
      this.sdata.parkArea = this.parkingListAll
      this.sdata.bikeNum = this.bikeList.length
      this.sdata.bikeArea = this.bikeListAll
      this.sdata.hdcdNum = this.activityList.length
      this.sdata.hdcdArea = this.activityListAll
      wx.setStorageSync('area', this.sdata)
      wx.navigateBack()
    },
    changeSwiper (event) {
      console.log(event.mp.detail)
      this.strollType = event.mp.detail.currentItemId
      this.current = event.mp.detail.current
    },
    onChange (index, type, e) {
      if (type === 'tingche') {
        const valueName = 'parking' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'zixingche') {
        const valueName = 'bike' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'huodong') {
        const valueName = 'activity' + index
        this[valueName] = e.mp.detail
      }
    },
    onChangeList (index, type, event) {
      if (type === 'tingche') {
        this.$set(this.parkingList, index, event.mp.detail)
      }
      if (type === 'zixingche') {
        this.$set(this.bikeList, index, event.mp.detail)
      }
      if (type === 'huodong') {
        this.$set(this.activityList, index, event.mp.detail)
      }
    },
    addList (type) {
      if (this.allgardenSum >= this.gardenArea) {
        wx.showToast({
          title: '已达上限',
          icon: 'none',
          duration: 2000
        })
        return
      }
      if (type === 'tingche') {
        this.$set(this.parkingList, this.parkingList.length, 0)
      }
      if (type === 'zixingche') {
        this.$set(this.bikeList, this.bikeList.length, 0)
      }
      if (type === 'huodong') {
        this.$set(this.activityList, this.activityList.length, 0)
      }
    },
    delList (type) {
      if (type === 'tingche') {
        this.$delete(this.parkingList, this.parkingList.length - 1)
      }
      if (type === 'zixingche') {
        this.$delete(this.bikeList, this.bikeList.length - 1)
      }
      if (type === 'huodong') {
        this.$delete(this.activityList, this.activityList.length - 1)
      }
    }
  },
  watch: {
    parkingList (val) {
      this.parkingListAll = 0
      for (let i = 0; i < this.parkingList.length; i++) {
        this.parkingListAll += this.parkingList[i]
      }
    },
    parkingListAll (val) {
      this.allgardenSum = this.parkingListAll + this.bikeListAll + this.activityListAll
      this.lastSum = ~~(this.gardenArea - this.allgardenSum)
    },
    bikeList (val) {
      this.bikeListAll = 0
      for (let i = 0; i < this.bikeList.length; i++) {
        this.bikeListAll += this.bikeList[i]
      }
    },
    bikeListAll (val) {
      this.allgardenSum = this.parkingListAll + this.bikeListAll + this.activityListAll
      this.lastSum = ~~(this.gardenArea - this.allgardenSum)
    },
    activityList (val) {
      this.activityListAll = 0
      for (let i = 0; i < this.activityList.length; i++) {
        this.activityListAll += this.activityList[i]
      }
    },
    activityListAll (val) {
      this.allgardenSum = this.parkingListAll + this.bikeListAll + this.activityListAll
      this.lastSum = ~~(this.gardenArea - this.allgardenSum)
    }
  },
  mounted () {
    // let app = getApp()
    // this.sdata = wx.getStorageSync('area')
    console.log(this.sdata)
    this.greenPer = this.sdata.greenPer.bottom
    this.peopleAbility = this.sdata.landArea / this.greenPer
    this.pavement = this.sdata.pavement
    this.pavementPercent = this.sdata.pavementPercent
    this.gardenArea = ~~(this.sdata.rateNum[3] / 100 * this.sdata.landArea)
    this.lastSum = this.gardenArea
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
  padding-top: 10px;
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
  width:200px;
}
.slide-image{
  width:134px;
  height:60px;
  margin: 0 10px;
}
.van-progress {
  width: 68%;
}
.swiper-per-body {
  min-height: 360px;
}
.delete {
  width: 20px;
  height: 20px;
  background: #ffffff;
  border: #5480FF 1px solid;
  border-radius: 11px;
  vertical-align: bottom;
  margin-right: 11px;
  color: #5480FF;
  font-size:15px;
  font-weight:400;
  line-height:18px;
  text-align: center;
}
</style>
