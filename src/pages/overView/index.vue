<template>
<div class="overview">
  <div class="overview-head">
    <div class="overview-head__title">场地总面积</div>
    <div class="overview-head__number">{{~~area.landArea + ~~area.waterArea}} <span class="m2">㎡</span> </div>
    <div class="overview-head__area-line">
      <div class="area-line__land-area">
        <p>{{area.landArea}}</p>
        <p>陆地面积(㎡)</p>
      </div>
      <div class="area-line__water-area">
        <p>{{area.waterArea}}</p>
        <p>水域面积(㎡)</p>
      </div>
    </div>
  </div>
  <div class="overview-body">
    <div class="chart-line">

      <div class="chart-line__block" @click="goPage('greenLand')">
        <div class="chart-line__block__title"><span>绿化用地(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>{{rateNum[0]}}</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit1" canvasId="chart1" />
          </div>
        </div>
        <div><span>{{greenLand}}</span>㎡</div>
      </div>
      <div class="chart-line__block" @click="goPage('strollRest')">
        <div class="chart-line__block__title"><span>游憩及服务(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/small.png" alt=""><span>{{rateNum[2]}}</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit2" canvasId="chart2" />
          </div>
        </div>
        <div><span>{{recreation}}</span>㎡</div>
      </div>
      
    </div>
    <div class="chart-line">

      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>管理建筑类(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon"><img src="/static/images/small.png" alt=""><span>{{rateNum[1]}}</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit3" canvasId="chart3" />
          </div>
        </div>
        <div><span>{{manager}}</span>㎡</div>
      </div>
      <div class="chart-line__block">
        <div class="chart-line__block__title"><span>园路及铺装(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon chart-line__icon--special">{{rateNum[3]}}-{{rateNum[4]}}</div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit4" canvasId="chart4" />
          </div>
        </div>
        <div><span>{{pavementBtm}}-{{pavementTop}}</span>㎡</div>
      </div>
      
    </div>
    
    <div class="ov__tab-bar">
      <div class="ov__tab-block ov__tab-block--left" :class="{'tab-block--active': leftActive}" @click="leftActive = true">人均绿地指标范围</div>
      <div class="ov__tab-block ov__tab-block--right" :class="{'tab-block--active': !leftActive}" @click="leftActive = false">建筑和数量</div>
    </div>

    <div class="ov__body" v-show="leftActive">

      <div class="ov__body__line">
        <div class="fl">
          <img src="/static/images/people.png" alt="" class="body-line__icon">
          <span>&nbsp;&nbsp;游人容量</span>
          <span v-if="peopleAbility">&nbsp;&nbsp;&nbsp;&nbsp;{{peopleAbility}}人</span>
        </div>
        <div class="fr">
          <div class="ov-body-line__spread fl">|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>
          <div class="ov-body-line__area fr">
            <p>{{peopleAbilityBottom}}人</p>
            <p>-</p>
            <p>{{peopleAbilityTop}}人</p>
          </div>
        </div>
      </div>
      <div class="ov__body__line ov__body__line--end">
        <div class="fl">
          <img src="/static/images/per.png" alt="" class="body-line__icon">
          <span>&nbsp;&nbsp;人均绿地</span>
          <span v-if="greenPer != 0">&nbsp;&nbsp;&nbsp;&nbsp;{{greenPer}}人</span>
        </div>
        <div class="fr">
          <div class="ov-body-line__spread fl">|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>
          <div class="ov-body-line__area fr">
            <p>{{greenPerBottom}}㎡</p>
            <p>-</p>
            <p>{{greenPerTop}}㎡</p>
          </div>
        </div>
      </div>
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
            <span class="icon-line--title--text">座椅</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>休息座椅数量</span>
            <span class="fr">{{seatNumBottom}}-{{seatNumTop}}个</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/images/car.png" alt="">
            <span class="icon-line--title--text">停车位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>停车位数量</span>
            <span class="fr">{{parkSpot}}个</span>
          </div>
          <div class="icon-line--body--line">
            <span>无障碍停车位数量</span>
            <span class="fr">{{parkSpotSpecial}}个</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/images/car.png" alt="">
            <span class="icon-line--title--text">自行车车位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>自行车车位需求数量</span>
            <span class="fr">{{bikeSpot}}个</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/images/car.png" alt="">
            <span class="icon-line--title--text">厕所蹲位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>累计厕所蹲位数量</span>
            <span class="fr">{{toiletBtm}}-{{toiletTop}}个</span>
          </div>
          <div class="icon-line--body--line">
            <span>男性蹲位数量</span>
            <span class="fr">{{manToiletBtm}}-{{manToiletTop}}个</span>
          </div>
          <div class="icon-line--body--line">
            <span>女性蹲位数量</span>
            <span class="fr">{{womanToiletBtm}}-{{womanToiletTop}}个</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/images/car.png" alt="">
            <span class="icon-line--title--text">路网体量参考</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>主路</span>
            <span class="fr">{{mainBtm}}-{{mainTop}}m</span>
          </div>
          <div class="icon-line--body--line">
            <span>次路</span>
            <span class="fr">{{minorBtm}}-{{minorTop}}m</span>
          </div>
          <div class="icon-line--body--line">
            <span>支路</span>
            <span class="fr">{{branchBtm}}-{{branchTop}}m</span>
          </div>
          <div class="icon-line--body--line">
            <span>小路</span>
            <span class="fr">{{pathBtm}}-{{pathTop}}m</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
    </div>

    <div class="ov__body" v-show="!leftActive">

      
      <!-- icon-line -->
      <div class="icon-line icon-line--right">
        <div class="green-title">
          游憩和服务类建筑
        </div>
        <div class="green-spread">
          <span class="green-spread-inner">服务类建筑</span> 
        </div>

        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="chart-edit" src="/static/images/edit.png" alt="">
            </div>
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>
        
        <div class="green-spread">
          <span class="green-spread-inner">游憩类建筑</span> 
        </div>

        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="chart-edit" src="/static/images/edit.png" alt="">
            </div>
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>

      </div>
      <!-- icon-line end -->

      <!-- icon-line -->
      <div class="icon-line icon-line--right">
        <div class="green-title">
          管理类建筑<img class="chart-edit" src="/static/images/edit.png" alt="">
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>
        
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <span class="icon-line--title--text">座椅</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>休息座椅数量</span>
              <span class="fr">666-555个</span>
            </div>
          </div>
        </div>

      </div>
      <!-- icon-line end -->

      <!-- icon-line -->
      <div class="icon-line icon-line--right">
        <div class="green-title">
          管理类建筑<img class="chart-edit" src="/static/images/edit.png" alt="">
        </div>
        <div class="icon-web">
          <div class="icon-web-line">

            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>

          </div>
          <div class="icon-web-line">

            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            <div class="icon-content">
              <img class="icon-line--title--icon" src="/static/images/seats.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>
            
          </div>
        </div>
        
      </div>
      <!-- icon-line end -->
      
    </div>

  </div>
</div>
  
</template>

<script>
import echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'

let chart = null

function getOption (num) {
  const rate = wx.getStorageSync('area').rateNum
  const rateNum = rate[num]
  return {
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
          {value: 100 - ~~rateNum},
          {value: ~~rateNum}
        ]
      }
    ]
  }
}
export default {
  components: {
    mpvueEcharts
  },
  data () {
    return {
      echarts,
      onInit1: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(0))
        return chart
      },
      onInit3: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(1))
        return chart
      },
      onInit2: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(2))
        return chart
      },
      onInit4: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(3))
        return chart
      },
      leftActive: true,
      area: {},
      rateNum: {},
      peopleAbility: 0,
      peopleAbilityBottom: 0,
      peopleAbilityTop: 0,
      greenPer: 0,
      greenPerBottom: 0,
      greenPerTop: 0,
      seatNumBottom: 0,
      seatNumTop: 0,
      parkSpot: 0,
      parkSpotSpecial: 0,
      bikeSpot: 0,
      toilet: 0,
      toiletBtm: 0,
      toiletTop: 0,
      manToilet: 0,
      manToiletBtm: 0,
      manToiletTop: 0,
      womanToiletBtm: 0,
      womanToiletTop: 0,
      womanToilet: 0,
      mainBtm: 0,
      mainTop: 0,
      minorBtm: 0,
      minorTop: 0,
      branchBtm: 0,
      branchTop: 0,
      pathBtm: 0,
      pathTop: 0,
      greenLand: 0,
      recreation: 0,
      manager: 0,
      pavementBtm: 0,
      pavementTop: 0
    }
  },
  methods: {
    goPage (pageName) {
      const url = `../${pageName}/main`
      console.log(url)
      mpvue.navigateTo({ url })
    }
  },
  mounted () {
    this.area = wx.getStorageSync('area')
    const rate = require('./rate.json')
    const la = ~~this.area.landArea
    const type = this.area.parkType
    switch (type) {
      case 'combine':
      case 'park':
        this.greenPerBottom = 30
        this.greenPerTop = 60
        this.peopleAbilityBottom = parseInt(la / 60)
        this.peopleAbilityTop = parseInt(la / 30)
        this.seatNumBottom = parseInt(this.peopleAbilityBottom * 0.2)
        this.seatNumTop = parseInt(this.peopleAbilityTop * 0.3)
        break
      case 'zoo':
      case 'plant':
      case 'other':
      case 'commity':
        this.greenPerBottom = 20
        this.greenPerTop = 30
        this.peopleAbilityBottom = parseInt(la / 30)
        this.peopleAbilityTop = parseInt(la / 20)
        this.seatNumBottom = parseInt(this.peopleAbilityBottom * 0.2)
        this.seatNumTop = parseInt(this.peopleAbilityTop * 0.3)
        break
    }
    if (la <= 19999) {
      this.rateNum = rate[0][type]
      this.parkSpot = ~~(la / 100 * 0.02)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 2
      this.mainTop = 4
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 1.2
      this.branchTop = 2
      this.pathBtm = 0.9
      this.pathTop = 1.2
    }
    if (la >= 20000 && la < 49999) {
      this.rateNum = rate[1][type]
      this.parkSpot = ~~(la / 100 * 0.02)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 2.5
      this.mainTop = 4.5
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 2
      this.branchTop = 2.5
      this.pathBtm = 0.9
      this.pathTop = 2
    }
    if (la >= 50000 && la < 99999) {
      this.rateNum = rate[2][type]
      this.parkSpot = ~~(la / 100 * 0.02)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 2.5
      this.mainTop = 4.5
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 2
      this.branchTop = 2.5
      this.pathBtm = 0.9
      this.pathTop = 2
    }
    if (la >= 100000 && la < 199999) {
      this.rateNum = rate[3][type]
      this.parkSpot = ~~(la / 100 * 0.05)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 4
      this.mainTop = 5
      this.minorBtm = 3
      this.minorTop = 4
      this.branchBtm = 2
      this.branchTop = 3
      this.pathBtm = 1.2
      this.pathTop = 2
    }
    if (la >= 200000 && la < 499999) {
      this.rateNum = rate[4][type]
      this.parkSpot = ~~(la / 100 * 0.05)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 4
      this.mainTop = 5
      this.minorBtm = 3
      this.minorTop = 4
      this.branchBtm = 2
      this.branchTop = 3
      this.pathBtm = 1.2
      this.pathTop = 2
    }
    if (la >= 500000 && la < 999999) {
      this.rateNum = rate[5][type]
      this.parkSpot = ~~(la / 100 * 0.08)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4
      this.mainTop = 7
      this.minorBtm = 3
      this.minorTop = 4
      this.branchBtm = 2
      this.branchTop = 3
      this.pathBtm = 1.2
      this.pathTop = 2
    }
    if (la >= 1000000 && la < 2999999) {
      this.rateNum = rate[6][type]
      this.parkSpot = ~~(la / 100 * 0.12)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4
      this.mainTop = 7
      this.minorBtm = 3
      this.minorTop = 4
      this.branchBtm = 2
      this.branchTop = 3
      this.pathBtm = 1.2
      this.pathTop = 2
    }
    if (la >= 3000000) {
      this.rateNum = rate[7][type]
      this.parkSpot = ~~(la / 100 * 0.12)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4
      this.mainTop = 7
      this.minorBtm = 3
      this.minorTop = 4
      this.branchBtm = 2
      this.branchTop = 3
      this.pathBtm = 1.2
      this.pathTop = 2
    }
    // 四大用地
    this.greenLand = ~~(~~this.rateNum[0] / 100 * la)
    this.recreation = ~~(~~this.rateNum[2] / 100 * la)
    this.manager = ~~(~~this.rateNum[1] / 100 * la)
    this.pavementBtm = ~~(~~this.rateNum[3] / 100 * la)
    this.pavementTop = ~~(~~this.rateNum[4] / 100 * la)

    // 无障碍停车位
    if (this.parkSpot < 100) {
      this.parkSpotSpecial = ~~(this.parkSpot / 50)
    } else {
      this.parkSpotSpecial = ~~(this.parkSpot * 0.02)
    }

    // 蹲位
    if (la < 100000) {
      this.toiletBtm = ~~(this.peopleAbilityBottom * 0.02)
      this.toiletTop = ~~(this.peopleAbilityTop * 0.02)
    } else {
      this.toiletBtm = ~~(this.peopleAbilityBottom * 0.15)
      this.toiletTop = ~~(this.peopleAbilityTop * 0.15)
    }
    this.manToiletBtm = ~~(this.toiletBtm * 0.4)
    this.manToiletTop = ~~(this.toiletTop * 0.4)
    this.womanToiletBtm = ~~(this.toiletBtm * 0.6)
    this.womanToiletTop = ~~(this.toiletTop * 0.6)

    this.area.rateNum = this.rateNum
    this.area.greenPer = {
      top: this.greenPerTop,
      bottom: this.greenPerBottom
    }
    this.area.peopleAbility = {
      top: this.peopleAbilityTop,
      bottom: this.peopleAbilityBottom
    }
    this.area.seatNum = {
      top: this.seatNumTop,
      bottom: this.seatNumBottom
    }
    wx.setStorageSync('area', this.area)
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
.chart-line__icon--special {
  font-size: 30px;
  height: 66px;
  line-height: 66px;
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
  color: #808080;
}
.ov-body-line__spread {
  color: #808080;
}
.ov__body__line--end {
  border-radius: 0 0 10px 10px;
}
</style>
