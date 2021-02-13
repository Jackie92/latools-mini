<template>
<div class="overview">
  <div class="overview-head">
    <div class="overview-head__title">{{area.parkType}}</div>
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
          <div class="chart-line__icon"><img src="/static/images/big.png" alt=""><span>{{rateNum[0]}}</span> <span class="small-percent">%</span> </div>
          <div class="echarts-wrap">
            <mpvue-echarts  v-if="JSON.stringify(rateNum)!=='{}'" :echarts="echarts" :onInit="onInit1" canvasId="chart1" />
          </div>
        </div>
        <div v-if="isChosePeople"><span>{{greenLand}}</span>㎡</div>
        <div v-else><span>{{0}}</span>㎡</div>
      </div>
      <div class="chart-line__block" @click="goPage('strollRest')">
        <div class="chart-line__block__title"><span>游憩及服务(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon" v-if="allFacSumRate === 0 || allFacSumRate == null"><img src="/static/images/small.png" alt=""><span>{{rateNum[2]}}</span><span class="small-percent">%</span></div>
          <div class="chart-line__icon" v-else><span>{{allFacSumRate}}</span><span class="small-percent">%</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit2" canvasId="chart2" />
          </div>
        </div>
        <!-- <div v-if="allFacSum !== 0"><span>{{recreation}}</span>㎡</div> -->
        <div><span>{{allFacSum}}</span>㎡</div>
      </div>
      
    </div>
    <div class="chart-line">

      <div class="chart-line__block" @click="goPage('manage')">
        <div class="chart-line__block__title"><span>管理建筑类(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon" v-if="allManaSumRate === 0 || allManaSumRate == null"><img src="/static/images/small.png" alt=""><span>{{rateNum[1]}}</span><span class="small-percent">%</span></div>
          <div class="chart-line__icon" v-else><span>{{allManaSumRate}}</span><span class="small-percent">%</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit3" canvasId="chart3" />
          </div>
        </div>
        <!-- <div><span>{{manager}}</span>㎡</div> -->
        <div><span>{{allManaSum}}</span>㎡</div>
      </div>
      <div class="chart-line__block" @click="goPage('garden')">
        <div class="chart-line__block__title"><span>园路及铺装(%)</span><img class="chart-edit" src="/static/images/edit.png" alt=""></div>
        <div class="chart-line">
          <div class="chart-line__icon chart-line__icon--special" v-if="allgardenSum === 0 || allgardenSum == null">{{rateNum[3]}}-{{rateNum[4]}}<span class="small-percent">%</span></div>
          <div class="chart-line__icon chart-line__icon--special" v-else>{{allgardenSumRate}}<span class="small-percent">%</span></div>
          <div class="echarts-wrap">
            <mpvue-echarts :echarts="echarts" :onInit="onInit4" canvasId="chart4" />
          </div>
        </div>
        <!-- <div><span>{{pavementBtm}}-{{pavementTop}}</span>㎡</div> -->
        <div><span>{{allgardenSum}}</span>㎡</div>
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
          <span>&nbsp;&nbsp;游人容量&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|</span>
        </div>
        <div class="fr">
          <div class="ov-body-line__spread fl"></div>
          <span class="fr">&nbsp;&nbsp;&nbsp;&nbsp;{{isChosePeople ? isNewPeople : '-/-'}}人</span>
          <div class="ov-body-line__area fr">
            <p>{{peopleAbilityBottom}}人</p>
            <p>-</p>
            <p>{{peopleAbilityTop}}人</p>
          </div>
          <!-- <span v-if="isChosePeople">&nbsp;&nbsp;&nbsp;&nbsp;{{isNewPeople}}人</span> -->
        </div>
      </div>

      <div class="ov__body__line ov__body__line--end">
        <div class="fl">
          <img src="/static/images/per.png" alt="" class="body-line__icon">
          <span>&nbsp;&nbsp;人均绿地&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|</span>
        </div>
        <div class="fr">
          <div class="ov-body-line__spread fl"></div>
          <span v-if="isChoseGreenPer">&nbsp;&nbsp;&nbsp;&nbsp;{{isNewGreenPer}}㎡</span>
          <div v-else class="ov-body-line__area fr">
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
            <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('greenLand')">
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
            <img class="icon-line--title--icon" src="/static/icon/car.png" alt="">
            <span class="icon-line--title--text">停车位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('garden')">
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
            <img class="icon-line--title--icon" src="/static/icon/bike.png" alt="">
            <span class="icon-line--title--text">自行车车位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('garden')">
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
            <img class="icon-line--title--icon" src="/static/icon/dw.png" alt="">
            <span class="icon-line--title--text">厕所蹲位</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('strollRest')">
          </div>
        </div>

        <div class="icon-line--body">
          <div class="icon-line--body--line">
            <span>累计厕所蹲位数量</span>
            <span class="fr" v-if="toiletChose === 0">{{toiletBtm}}-{{toiletTop}}个</span>
            <span class="fr" v-else>{{toiletChose}}个</span>
          </div>
          <div class="icon-line--body--line">
            <span>男性蹲位数量</span>
            <span class="fr" v-if="manToiletChose === 0">{{manToiletBtm}}-{{manToiletTop}}个</span>
            <span class="fr" v-else>{{manToiletChose}}个</span>
          </div>
          <div class="icon-line--body--line">
            <span>女性蹲位数量</span>
            <span class="fr" v-if="womanToiletChose === 0">{{womanToiletBtm}}-{{womanToiletTop}}个</span>
            <span class="fr" v-else>{{womanToiletChose}}个</span>
          </div>
        </div>

      </div>
      <!-- icon-line end -->
      <!-- icon-line -->
      <div class="icon-line">

        <div class="icon-line--title">
          <div class="icon-line--title--line">
            <img class="icon-line--title--icon" src="/static/icon/road.png" alt="">
            <span class="icon-line--title--text">路网体量参考</span>
            <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('strollRest')">
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
              <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('strollRest')">
            </div>
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/cs.png" alt="">
              <span class="icon-line--title--text">厕所</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{csArea === 0 ? '-/-' : csArea}}㎡</span>
              <span class="fr">{{csNum === 0 ? '-/-' : csNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/medical.png" alt="">
              <span class="icon-line--title--text">医疗救助站</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{ylArea === 0 ? '-/-' : ylArea}}㎡</span>
              <span class="fr">{{ylNum === 0 ? '-/-' : ylNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/xmb.png" alt="">
              <span class="icon-line--title--text">小卖部</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{xmbArea === 0 ? '-/-' : xmbArea}}㎡</span>
              <span class="fr">{{xmbNum === 0 ? '-/-' : xmbNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/cafe.png" alt="">
              <span class="icon-line--title--text">咖啡和茶座</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{cafeArea === 0 ? '-/-' : cafeArea}}㎡</span>
              <span class="fr">{{cafeNum === 0 ? '-/-' : cafeNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/yk.png" alt="">
              <span class="icon-line--title--text">游客服务中心</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{ykArea === 0 ? '-/-' : ykArea}}㎡</span>
              <span class="fr">{{ykNum === 0 ? '-/-' : ykNum}}座</span>
            </div>
          </div>
        </div>
        
        <div class="green-spread">
          <span class="green-spread-inner">游憩类建筑</span> 
        </div>

        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('strollRest')">
            </div>
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/tltx.png" alt="">
              <span class="icon-line--title--text">亭廊厅榭</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{tltxArea === 0 ? '-/-' : tltxArea}}㎡</span>
              <span class="fr">{{tltxNum === 0 ? '-/-' : tltxNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/hd.png" alt="">
              <span class="icon-line--title--text">活动馆</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{hdgArea === 0 ? '-/-' : hdgArea}}㎡</span>
              <span class="fr">{{hdgNum === 0 ? '-/-' : hdgNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/zl.png" alt="">
              <span class="icon-line--title--text">展览馆</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{zlgArea === 0 ? '-/-' : zlgArea}}㎡</span>
              <span class="fr">{{zlgNum === 0 ? '-/-' : zlgNum}}座</span>
            </div>
          </div>
        </div>

      </div>
      <!-- icon-line end -->

      <!-- icon-line -->
      <div class="icon-line icon-line--right">
        <div class="green-title">
          管理类建筑<img class="chart-edit" src="/static/images/edit.png" alt="" @click="goPage('manage')">
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/abjk.png" alt="">
              <span class="icon-line--title--text">安保监控室</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{abjkArea === 0 ? '-/-' : abjkArea}}㎡</span>
              <span class="fr">{{abjkNum === 0 ? '-/-' : abjkNum}}座</span>
            </div>
          </div>
        </div>
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/gl.png" alt="">
              <span class="icon-line--title--text">管理办公室</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{manageArea === 0 ? '-/-' : manageArea}}㎡</span>
              <span class="fr">{{manageNum === 0 ? '-/-' : manageNum}}座</span>
            </div>
          </div>
        </div>
        
        <div class="icon-wrapper">
          <div class="icon-line--title">
            <div class="icon-line--title--line">
              <img class="icon-line--title--icon" src="/static/icon/gb.png" alt="">
              <span class="icon-line--title--text">广播室</span>
            </div>
          </div>
          <div class="icon-line--body">
            <div class="icon-line--body--line">
              <span>面积 {{gbsArea === 0 ? '-/-' : gbsArea}}㎡</span>
              <span class="fr">{{gbsNum === 0 ? '-/-' : gbsNum}}座</span>
            </div>
          </div>
        </div>

      </div>
      <!-- icon-line end -->

      <!-- icon-line -->
      <div class="icon-line icon-line--right">
        <div class="green-title">
          其他设施<img class="chart-edit" src="/static/images/edit.png" alt="">
        </div>
        <div class="icon-web">
          <div class="icon-web-line">

            <div class="icon-content">
              <img v-if="facility.manage[4] === 2" class="icon-line--title--icon" src="/static/icon/bf.png" alt="">
              <img v-else-if="facility.manage[4] === 1" class="icon-line--title--icon" src="/static/icon/bf3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/bf2.png" alt="">
              <div class="icon-describe">泵房</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manage[3] === 2" class="icon-line--title--icon" src="/static/icon/bpd.png" alt="">
              <img v-else-if="facility.manage[3] === 1" class="icon-line--title--icon" src="/static/icon/bpd3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/bpd2.png" alt="">
              <div class="icon-describe">变配电所</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manage[1] === 2" class="icon-line--title--icon" src="/static/icon/ljzz.png" alt="">
              <img v-else-if="facility.manage[1] === 1" class="icon-line--title--icon" src="/static/icon/ljzz3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/ljzz2.png" alt="">
              <div class="icon-describe">垃圾中转站</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manage[0] === 2" class="icon-line--title--icon" src="/static/icon/wq.png" alt="">
              <img v-else-if="facility.manage[0] === 1" class="icon-line--title--icon" src="/static/icon/wq3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/wq2.png" alt="">
              <div class="icon-describe">围墙围栏</div>
            </div>

          </div>
          <div class="icon-web-line">

            <div class="icon-content">
              <img v-if="facility.manage[2] === 2" class="icon-line--title--icon" src="/static/icon/lslj.png" alt="">
              <img v-else-if="facility.manage[2] === 1" class="icon-line--title--icon" src="/static/icon/lslj3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/lslj2.png" alt="">
              <div class="icon-describe">绿色垃圾处理站</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manage[5] === 2" class="icon-line--title--icon" src="/static/icon/ws.png" alt="">
              <img v-else-if="facility.manage[5] === 1" class="icon-line--title--icon" src="/static/icon/ws3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/ws2.png" alt="">
              <div class="icon-describe">温室荫棚</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manageFacilities[0] === 2" class="icon-line--title--icon" src="/static/icon/yj.png" alt="">
              <img v-else-if="facility.manageFacilities[0] === 1" class="icon-line--title--icon" src="/static/icon/yj3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/yj2.png" alt="">
              <div class="icon-describe">应急避险处理站</div>
            </div>
            <div class="icon-content">
              <img v-if="facility.manageFacilities[1] === 2" class="icon-line--title--icon" src="/static/icon/yskz.png" alt="">
              <img v-else-if="facility.manageFacilities[1] === 1" class="icon-line--title--icon" src="/static/icon/yskz3.png" alt="">
              <img v-else class="icon-line--title--icon" src="/static/icon/yskz2.png" alt="">
              <div class="icon-describe">雨水控制</div>
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
import { mapState } from 'vuex'
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
        console.log('oninit1')
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
      isChosePeople: false,
      isChoseGreenPer: false,
      isNewGreenPer: 0,
      isNewPeople: 0,
      reloadFlag: true,
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
      pavementTop: 0,
      hdgNum: 0,
      hdgArea: 0,
      zlgNum: 0,
      zlgArea: 0,
      tltxNum: 0,
      tltxArea: 0,
      csNum: 0,
      csArea: 0,
      csdwNum: 0,
      ykArea: 0,
      ykNum: 0,
      xmbArea: 0,
      xmbNum: 0,
      cafeArea: 0,
      cafeNum: 0,
      ylArea: 0,
      ylNum: 0,
      toiletChose: 0,
      manToiletChose: 0,
      womanToiletChose: 0,
      abjkNum: 0,
      abjkArea: 0,
      manageNum: 0,
      manageArea: 0,
      gbsNum: 0,
      gbsArea: 0,
      facility: null,
      allFacSum: 0,
      allFacSumRate: 0,
      allManaSum: 0,
      allManaSumRate: 0,
      allgardenSum: 0,
      allgardenSumRate: 0
    }
  },
  computed: mapState(['sdata']),
  methods: {
    goPage (pageName) {
      const url = `../subPackage/${pageName}/main`
      console.log(url)
      mpvue.navigateTo({ url })
    },
    numFilter (value) {
      console.log(value)
      let realVal = ''
      if (!isNaN(value) && value !== '') {
        // 截取当前数据到小数点后两位
        realVal = parseFloat(value).toFixed(1)
        let xsd = realVal.toString().split('.')
        if (xsd.length === 1) {
          realVal = realVal.toString() + '.0'
        }
      } else {
        realVal = '-/-'
      }
      return realVal
    }
  },
  mounted () {
    this.area = wx.getStorageSync('area')
    const rate = require('./rate.json')
    const facility = require('../facility.json')
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
      this.mainBtm = 2.0
      this.mainTop = 4.0
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 1.2
      this.branchTop = 2.0
      this.pathBtm = 0.9
      this.pathTop = 1.2
      this.area.facility = facility[0]
    }
    if (la >= 20000 && la < 49999) {
      this.rateNum = rate[1][type]
      this.parkSpot = ~~(la / 100 * 0.02)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 2.5
      this.mainTop = 4.5
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 2.0
      this.branchTop = 2.5
      this.pathBtm = 0.9
      this.pathTop = 2.0
      this.area.facility = facility[1]
    }
    if (la >= 50000 && la < 99999) {
      this.rateNum = rate[2][type]
      this.parkSpot = ~~(la / 100 * 0.02)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 2.5
      this.mainTop = 4.5
      this.minorBtm = '-'
      this.minorTop = '-'
      this.branchBtm = 2.0
      this.branchTop = 2.5
      this.pathBtm = 0.9
      this.pathTop = 2.0
      this.area.facility = facility[2]
    }
    if (la >= 100000 && la < 199999) {
      this.rateNum = rate[3][type]
      this.parkSpot = ~~(la / 100 * 0.05)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 4.0
      this.mainTop = 5.0
      this.minorBtm = 3.0
      this.minorTop = 4.0
      this.branchBtm = 2.0
      this.branchTop = 3.0
      this.pathBtm = 1.2
      this.pathTop = 2.0
      this.area.facility = facility[3]
    }
    if (la >= 200000 && la < 499999) {
      this.rateNum = rate[4][type]
      this.parkSpot = ~~(la / 100 * 0.05)
      this.bikeSpot = ~~(la / 100 * 0.5)
      this.mainBtm = 4.0
      this.mainTop = 5.0
      this.minorBtm = 3.0
      this.minorTop = 4.0
      this.branchBtm = 2.0
      this.branchTop = 3.0
      this.pathBtm = 1.2
      this.pathTop = 2.0
      this.area.facility = facility[4]
    }
    if (la >= 500000 && la < 999999) {
      this.rateNum = rate[5][type]
      this.parkSpot = ~~(la / 100 * 0.08)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4.0
      this.mainTop = 7.0
      this.minorBtm = 3.0
      this.minorTop = 4.0
      this.branchBtm = 2.0
      this.branchTop = 3.0
      this.pathBtm = 1.2
      this.pathTop = 2.0
      this.area.facility = facility[5]
    }
    if (la >= 1000000 && la < 2999999) {
      this.rateNum = rate[6][type]
      this.parkSpot = ~~(la / 100 * 0.12)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4.0
      this.mainTop = 7.0
      this.minorBtm = 3.0
      this.minorTop = 4.0
      this.branchBtm = 2.0
      this.branchTop = 3.0
      this.pathBtm = 1.2
      this.pathTop = 2.0
      this.area.facility = facility[6]
    }
    if (la >= 3000000) {
      this.rateNum = rate[7][type]
      this.parkSpot = ~~(la / 100 * 0.12)
      this.bikeSpot = ~~(la / 100 * 0.2)
      this.mainBtm = 4.0
      this.mainTop = 7.0
      this.minorBtm = 3.0
      this.minorTop = 4.0
      this.branchBtm = 2.0
      this.branchTop = 3.0
      this.pathBtm = 1.2
      this.pathTop = 2.0
      this.area.facility = facility[7]
    }
    this.facility = this.area.facility
    this.mainBtm = this.numFilter(this.mainBtm)
    this.mainTop = this.numFilter(this.mainTop)
    this.minorBtm = this.numFilter(this.minorBtm)
    this.minorTop = this.numFilter(this.minorTop)
    this.branchBtm = this.numFilter(this.branchBtm)
    this.branchTop = this.numFilter(this.branchTop)
    this.pathBtm = this.numFilter(this.pathBtm)
    this.pathTop = this.numFilter(this.pathTop)
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
      bottom: this.greenPerBottom,
      chose: this.greenPerBottom,
      isChose: false
    }
    this.area.peopleAbility = {
      top: this.peopleAbilityTop,
      bottom: this.peopleAbilityBottom,
      chose: this.peopleAbilityBottom,
      isChose: false
    }
    this.area.seatNum = {
      top: this.seatNumTop,
      bottom: this.seatNumBottom
    }
    this.area.toiletBtm = this.toiletBtm
    this.area.toiletTop = this.toiletTop
    this.area.manToiletBtm = this.manToiletBtm
    this.area.manToiletTop = this.manToiletTop
    this.area.womanToiletBtm = this.womanToiletBtm
    this.area.womanToiletTop = this.womanToiletTop
    this.area.pavement = this.pavementBtm
    this.area.pavementPercent = this.rateNum[3]
    // 四大用地存储
    // 四大用地
    this.area.limitgreenLand = ~~(~~this.rateNum[0] / 100 * la)
    this.area.limitrecreation = ~~(~~this.rateNum[2] / 100 * la)
    this.area.limitmanager = ~~(~~this.rateNum[1] / 100 * la)
    this.area.limitpavementBtm = ~~(~~this.rateNum[3] / 100 * la)
    this.area.limitpavementTop = ~~(~~this.rateNum[4] / 100 * la)
    // wx.setStorageSync('area', this.area)
    this.$store.commit('_setData', this.area)
    wx.setStorageSync('area', this.area)
    console.log('$store2', this.$store.state.sdata)
    switch (this.area.parkType) {
      case 'combine':
        this.area.parkType = '综合公园'
        break
      case 'community':
        this.area.parkType = '社区公园'
        break
      case 'other':
        this.area.parkType = '其他专类园'
        break
      case 'park':
        this.area.parkType = '游园'
        break
      case 'plant':
        this.area.parkType = '植物园'
        break
      case 'zoo':
        this.area.parkType = '动物园'
        break
    }
  },
  onShow () {
    console.log('$storeOnShow', this.$store.state.sdata)
    let _data = this.$store.state.sdata
    console.log('$storeOnShow', _data.peopleAbility)
    if (_data.peopleAbility !== undefined) {
      this.isChosePeople = _data.peopleAbility.isChose
      this.isChoseGreenPer = _data.greenPer.isChose
      this.isNewPeople = _data.peopleAbility.chose
      this.isNewGreenPer = _data.greenPer.chose
      this.hdgNum = _data.hdgNum
      this.hdgArea = _data.hdgArea
      this.zlgNum = _data.zlgNum
      this.zlgArea = _data.zlgArea
      this.tltxNum = _data.tltxNum
      this.tltxArea = _data.tltxArea
      this.csNum = _data.csNum
      this.csArea = _data.csArea
      this.csdwNum = _data.csdwNum
      this.ykArea = _data.ykArea
      this.ykNum = _data.ykNum
      this.xmbArea = _data.xmbArea
      this.xmbNum = _data.xmbNum
      this.cafeArea = _data.cafeArea
      this.cafeNum = _data.cafeNum
      this.ylArea = _data.ylArea
      this.ylNum = _data.ylNum
      this.toiletChose = _data.toiletChose !== undefined ? _data.toiletChose : 0
      this.manToiletChose = _data.manToiletChose !== undefined ? _data.manToiletChose : 0
      this.womanToiletChose = _data.womanToiletChose !== undefined ? _data.womanToiletChose : 0
      this.abjkNum = _data.abjkNum
      this.abjkArea = _data.abjkArea
      this.manageNum = _data.manageNum
      this.manageArea = _data.manageArea
      this.gbsNum = _data.gbsNum
      this.gbsArea = _data.gbsArea
      this.allFacSum = _data.allFacSum == null ? 0 : _data.allFacSum
      this.allFacSumRate = _data.allFacSumRate == null ? 0 : _data.allFacSumRate
      this.allManaSum = _data.allManaSum == null ? 0 : _data.allManaSum
      this.allManaSumRate = _data.allManaSumRate == null ? 0 : _data.allManaSumRate
      this.allgardenSum = _data.allgardenSum == null ? 0 : _data.allgardenSum
      this.allgardenSumRate = _data.allgardenSumRate == null ? 0 : _data.allgardenSumRate
    }
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
  font-size: 30px;
  color: #5480ff;
  float: left;
  padding-top: 10px;
}
.chart-line__icon--special {
  font-size: 26px;
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
  color: #b1b0b0;
}
.ov-body-line__spread {
  color: #b1b0b0;
}
.ov__body__line--end {
  border-radius: 0 0 10px 10px;
}
.small-percent {
  font-size: 12px;
}
</style>
