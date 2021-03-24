<template>
<div class="gl">
  <div class="gl-head">
    <div class="fl">
      <div class="gl-head__title">游憩服务用地</div>
      <div class="gl-head__number">{{rateNumNew}}<span class="m2">%</span> </div>
      <div class="gl-head__area-line">
        <div class="area-line__land-area">
          <p>已用：{{allFacSum}}㎡</p>
          <p>上限：{{sdata.limitrecreation}}㎡</p>
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
    <!-- 游憩类start -->
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

      <!-- 游憩类-活动馆start -->
      <div v-if="strollType === 'huodong'" class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.recreationArch[1] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.recreationArch[1] === 2" @click="showHint(2)">
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
                <img class="greenarea" src="/static/icon/hd.png" alt="">活动馆用地
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
                  <div class="number">{{i + 1}}</div>活动馆{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='activityList[i]'
                  @change="onChangeList(i, 'huodong', $event)"
                  :min='0'
                  :max='lastMax > 2000 ? 2000 : lastMax'
                />
                <div class="green-pro-left">{{activityList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add"  v-if="(sdata.facility.recreationArch[1] !== 0 && index1 !== 0) || sdata.facility.recreationArch[1] === 2"><div class="number"  @click="addList('huodong')">+</div><div class="delete" @click="delList('huodong')">-</div></div>
          <div class="add"  v-else><div class="number" >+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 游憩类-活动馆end -->
      
      <!-- 游憩类-廊亭厅榭start -->
      <div v-if="strollType === 'tinglang'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.recreationArch[0] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.recreationArch[0] === 2" @click="showHint(2)">
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
                <img class="greenarea" src="/static/icon/tltx.png" alt="">亭廊厅榭用地
              </div>
              <div class="bet-item-right">{{galleryList.length}}个</div>
            </div>
            <div class="bet-item-be">{{galleryListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(galleryListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(galleryListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="galleryList.length > 0" v-for="(item, i) in galleryList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>亭廊厅榭{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='galleryList[i]'
                  @change="onChangeList(i, 'tinglang', $event)"
                  :min='0'
                  :max='lastMax > 500 ? 500 : lastMax'
                />
                <div class="green-pro-left">{{galleryList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.recreationArch[0] !== 0 && index2 !== 0) || sdata.facility.recreationArch[0] === 2"><div class="number"  @click="addList('tinglang')">+</div><div class="delete" @click="delList('tinglang')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 游憩类-廊亭厅榭end -->

      <!-- 游憩类-展览馆start -->
      <div v-if="strollType === 'zhanlan'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.recreationArch[2] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.recreationArch[2] === 2" @click="showHint(2)">
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
                <img class="greenarea" src="/static/icon/zl.png" alt="">展览馆用地
              </div>
              <div class="bet-item-right">{{exhibitionList.length}}个</div>
            </div>
            <div class="bet-item-be">{{exhibitionListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(exhibitionListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(exhibitionListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="exhibitionList.length > 0" v-for="(item, i) in exhibitionList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>展览馆{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='exhibitionList[i]'
                  @change="onChangeList(i, 'zhanlan', $event)"
                  :min='0'
                  :max='lastMax'
                />
                <div class="green-pro-left">{{exhibitionList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.recreationArch[2] !== 0 && index3 !== 0) || sdata.facility.recreationArch[2] === 2"><div class="number"  @click="addList('zhanlan')">+</div><div class="delete" @click="delList('zhanlan')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
        </div>
      </div>
      <!-- 游憩类-展览馆end -->

    </div>
    <!-- 游憩类end -->

    <!-- 建筑类start -->
    <div class="gl-showBody" v-else>
      <div class="swiper area-swiper">
        <swiper
          class="swiper-inner"
          :indicator-dots='false'
          :autoplay='false'
          :interval='3000'
          :circular='circular'
          :previous-margin="'100px'"
          :next-margin="'100px'"
          :current='current2'
          @change='changeSwiper2'
        >
          <a  v-for='(img, index) in topSwipers2' :key='index'>

            <swiper-item class="swiper-bar" :item-id='img.id'>
              <img              
                v-if="current2 === index"
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

      <!-- 建筑类-厕所start -->
      <div v-if="strollType2 === 'cs'" class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serveArch[1] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serveArch[1] === 2" @click="showHint(2)">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index4=0" :class="index4==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index4=1" :class="index4==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/cs.png" alt="">厕所用地
              </div>
              <div class="bet-item-right">{{toiletList.length}}个</div>
            </div>
            <div class="bet-item-be">{{toiletListAll}}㎡</div>
            <div class="green-pro-one">
              <van-progress
                class="van-progress"
                :pivot-text="toiletListAll > toiletAreaLine ? '✔' : toiletListAll"
                color="#5380FF"
                show-pivot
                :percentage="~~(toiletListAll / toiletAreaLine * 100) > 100 ? 100 : ~~(toiletListAll / toiletAreaLine * 100 + 3)"
              />
              <div class="green-pro-left">{{(toiletListAll > toiletAreaLine ? (toiletAreaLine / sdata.landArea * 100 ) : (toiletListAll / sdata.landArea * 100 )) | numFilter}}%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/dw.png" alt="">蹲位数量
              </div>
            </div>
            <div class="green-pro-one">
              <!-- <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress> -->
              <van-progress
                class="van-progress"
                :pivot-text="toiletNum > toiletNumLine ? '✔' : toiletNum"
                color="#5380FF"
                show-pivot
                :percentage="~~(toiletNum / toiletNumLine * 100) > 100 ? 100 : ~~(toiletNum / toiletNumLine * 100) + 3"
              />
              <div class="green-pro-left">{{toiletNum > toiletNumLine ? toiletNumLine : toiletNum}}个</div>
            </div>
          </div>
          <div v-if="toiletList.length > 0" v-for="(item, i) in toiletList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>厕所{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='toiletList[i]'
                  @change="onChangeList(i, 'cs', $event)"
                  :min='0'
                  :max='lastMax > 2000 ? 2000 : lastMax'
                />
                <div class="green-pro-left">{{toiletList[i]}}㎡</div>
              </div>
              <div class="arrow-toilet" v-if="showToilet[i] == null || showToilet[i] == false" @click="showToiletFun(i, true)">可设置详情数据</div>
              <div v-show="showToilet[i]" class="small-percent" style="padding-top: 20px">
                <div class="bet-item">
                  <div class="bet-item-title fl">
                    <div class="bet-item-left">
                      单位蹲位面积：
                    </div>
                  </div>
                  <div class="green-pro green-pro-special">
                    <van-slider
                      class="greenslider"
                      bar-height="3.1px"
                      active-color="#5380FF"
                      inactive-color="#EBEBEB"
                      :value='toiletListDetail0[i] == null ? 6 : toiletListDetail0[i]'
                      @change="onChangeList(i, 'csxq0', $event)"
                      :min='0'
                      :max='20'
                      />
                  <div class="green-pro-left">{{toiletListDetail0[i] == null ? 6 : toiletListDetail0[i]}}㎡</div>
                  </div>
                </div>
                <div class="bet-item">
                  <div class="bet-item-title fl">
                    <div class="bet-item-left">
                      管理房面积：&nbsp;&nbsp;&nbsp;
                    </div>
                  </div>
                  <div class="green-pro green-pro-special">
                    <van-slider
                      class="greenslider"
                      bar-height="3.1px"
                      active-color="#5380FF"
                      inactive-color="#EBEBEB"
                      :value='toiletListDetail1[i] == null ? 6 : toiletListDetail1[i]'
                      @change="onChangeList(i, 'csxq1', $event)"
                      :min='6'
                      :max='20'
                      />
                  <div class="green-pro-left">{{toiletListDetail1[i] == null ? 6 : toiletListDetail1[i]}}㎡</div>
                  </div>
                </div> 
                <div class="bet-item">
                  <div class="bet-item-title fl">
                    <div class="bet-item-left">
                      工具房面积：&nbsp;&nbsp;&nbsp;
                    </div>
                  </div>
                  <div class="green-pro green-pro-special">
                    <van-slider
                      class="greenslider"
                      bar-height="3.1px"
                      active-color="#5380FF"
                      inactive-color="#EBEBEB"
                      :value='toiletListDetail2[i] == null ? 2 : toiletListDetail2[i]'
                      @change="onChangeList(i, 'csxq2', $event)"
                      :min='2'
                      :max='20'
                      />
                  <div class="green-pro-left">{{toiletListDetail2[i] == null ? 2 : toiletListDetail2[i]}}㎡</div>
                  </div>
                </div>
                <div class="bet-item">
                  <div class="bet-item-title fl">
                    <div class="bet-item-left">
                      第三卫生间：&nbsp;&nbsp;&nbsp;
                    </div>
                  </div>
                  <div class="green-pro green-pro-special">
                    <van-slider
                      class="greenslider"
                      bar-height="3.1px"
                      active-color="#5380FF"
                      inactive-color="#EBEBEB"
                      :value='toiletListDetail3[i] == null ? 6 : toiletListDetail3[i]'
                      @change="onChangeList(i, 'csxq3', $event)"
                      :min='6'
                      :max='20'
                      />
                  <div class="green-pro-left">{{toiletListDetail3[i] == null ? 6 : toiletListDetail3[i]}}㎡</div>
                  </div>
                </div>
              </div>
              <div class="arrow-toilet" v-if="showToilet[i] == true" @click="showToiletFun(i, false)">收起详情</div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.serveArch[1] !== 0 && index4 !== 0) || sdata.facility.serveArch[1] === 2"><div class="number"  @click="addList('cs')">+</div><div class="delete" @click="delList('cs')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
         </div>
      </div>
      <!-- 建筑类-厕所end -->
      
      <!-- 建筑类-游客服务中心start -->
      <div v-if="strollType2 === 'yk'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serveArch[0] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serveArch[0] === 2" @click="showHint(2)">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index5=0" :class="index5==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index5=1" :class="index5==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/yk.png" alt="">游客服务中心用地
              </div>
              <div class="bet-item-right">{{visitorList.length}}个</div>
            </div>
            <div class="bet-item-be">{{visitorListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(visitorListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(visitorListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="visitorList.length > 0" v-for="(item, i) in visitorList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>游客服务中心{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='visitorList[i]'
                  @change="onChangeList(i, 'yk', $event)"
                  :min='0'
                  :max='lastMax'
                />
                <div class="green-pro-left">{{visitorList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.serveArch[0] !== 0 && index5 !== 0) || sdata.facility.serveArch[0] === 2"><div class="number"  @click="addList('yk')">+</div><div class="delete" @click="delList('yk')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
         </div>
      </div>
      <!-- 建筑类-游客服务中心end -->

      <!-- 建筑类-茶座和咖啡start -->
      <div v-if="strollType2 === 'cz'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serveArch[4] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serveArch[4] === 2" @click="showHint(2)">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index6=0" :class="index6==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index6=1" :class="index6==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/cafe.png" alt="">茶座/咖啡厅用地
              </div>
              <div class="bet-item-right">{{cafeList.length}}个</div>
            </div>
            <div class="bet-item-be">{{cafeListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(cafeListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(cafeListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="cafeList.length > 0" v-for="(item, i) in cafeList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>茶座/咖啡厅{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='cafeList[i]'
                  @change="onChangeList(i, 'cafe', $event)"
                  :min='0'
                  :max='lastMax'
                />
                <div class="green-pro-left">{{cafeList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.serveArch[4] !== 0 && index6 !== 0) || sdata.facility.serveArch[4] === 2"><div class="number"  @click="addList('cafe')">+</div><div class="delete" @click="delList('cafe')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
          </div>
      </div>
      <!-- 建筑类-茶座和咖啡end -->

      <!-- 建筑类-医疗救助站start -->
      <div v-if="strollType2 === 'yl'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serveArch[6] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serveArch[6] === 2" @click="showHint(2)">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index7=0" :class="index7==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index7=1" :class="index7==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/medical.png" alt="">医疗救助站用地
              </div>
              <div class="bet-item-right">{{medicalList.length}}个</div>
            </div>
            <div class="bet-item-be">{{medicalListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(medicalListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(medicalListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="medicalList.length > 0" v-for="(item, i) in medicalList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>医疗救助站{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='medicalList[i]'
                  @change="onChangeList(i, 'yl', $event)"
                  :min='0'
                  :max='lastMax'
                />
                <div class="green-pro-left">{{medicalList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.serveArch[6] !== 0 && index7 !== 0) || sdata.facility.serveArch[6] === 2"><div class="number"  @click="addList('yl')">+</div><div class="delete" @click="delList('yl')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
          </div>
      </div>
      <!-- 建筑类-医疗救助站end -->

      
      <!-- 建筑类-小卖部start -->
      <div v-if="strollType2 === 'xmb'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.facility.serveArch[5] === 0" @click="showHint(1)">
            <div class="switch-item current" >off</div>
            <div class="switch-item">on</div>
          </div>
          <div class="three-switch" v-else-if="sdata.facility.serveArch[5] === 2" @click="showHint(2)">
            <div class="switch-item" >off</div>
            <div class="switch-item current">on</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index8=0" :class="index8==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index8=1" :class="index8==1 ? 'current':''">on</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/xmb.png" alt="">小卖部用地
              </div>
              <div class="bet-item-right">{{shopList.length}}个</div>
            </div>
            <div class="bet-item-be">{{shopListAll}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" :percent="(shopListAll / sdata.landArea * 100) | numFilter" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">{{(shopListAll / sdata.landArea * 100) | numFilter}}%</div>
            </div>
          </div>
          <div v-if="shopList.length > 0" v-for="(item, i) in shopList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 1}}</div>小卖部{{i + 1}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  :value='shopList[i]'
                  @change="onChangeList(i, 'xmb', $event)"
                  :min='0'
                  :max='lastMax'
                />
                <div class="green-pro-left">{{shopList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" v-if="(sdata.facility.serveArch[5] !== 0 && index8 !== 0) || sdata.facility.serveArch[5] === 2"><div class="number"  @click="addList('xmb')">+</div><div class="delete" @click="delList('xmb')">-</div></div>
          <div class="add" v-else><div class="number">+</div><div class="delete" >-</div></div>
         </div>
      </div>
      <!-- 建筑类-小卖部end -->
    </div>
    <!-- 建筑类end -->
    
    <div class="submit" @click="submit">提交修改</div>
  </div>
</div>
  
</template>

<script>
import echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'
import { mapState } from 'vuex'

let chart = null
// const rate = wx.getStorageSync('area').rateNum
// let rateNum = rate[2]
let rateNum = 0
function getOption (num) {
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
    ]
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
      chart,
      onInit: function (canvas, width, height) {
        chart = echarts.init(canvas, null, {
          width: width,
          height: height
        })
        canvas.setChart(chart)
        chart.setOption(getOption(2))
        return chart
      },
      typeIn: 0,
      topSwipers: [{
        id: 'huodong',
        outterImage: '../../../static/images/hdg.png',
        outterImageCur: '../../../static/images/hdg2.png'
      },
      {
        id: 'zhanlan',
        outterImage: '../../../static/images/zlg.png',
        outterImageCur: '../../../static/images/zlg2.png'
      },
      {
        id: 'tinglang',
        outterImage: '../../../static/images/tltx.png',
        outterImageCur: '../../../static/images/tltx2.png'
      }],
      topSwipers2: [{
        id: 'cs',
        outterImage: '../../../static/images/cs.png',
        outterImageCur: '../../../static/images/cs2.png'
      },
      {
        id: 'yk',
        outterImage: '../../../static/images/yk.png',
        outterImageCur: '../../../static/images/yk2.png'
      },
      {
        id: 'xmb',
        outterImage: '../../../static/images/xmb.png',
        outterImageCur: '../../../static/images/xmb2.png'
      },
      {
        id: 'cz',
        outterImage: '../../../static/images/cz.png',
        outterImageCur: '../../../static/images/cz2.png'
      },
      {
        id: 'yl',
        outterImage: '../../../static/images/yl.png',
        outterImageCur: '../../../static/images/yl2.png'
      }],
      // sdata: {},
      greenPer: 0,
      peopleAbility: 0,
      strollType: 'huodong',
      strollType2: 'cs',
      activity1: 150,
      activity2: 150,
      activityList: [],
      activityListAll: 0,
      gallery1: 20,
      gallery2: 20,
      galleryList: [],
      galleryListAll: 0,
      exhibition1: 200,
      exhibition2: 300,
      exhibitionList: [],
      exhibitionListAll: 0,
      toilet1: 150,
      toilet2: 150,
      toiletList: [],
      toiletListAll: 0,
      visitor1: 100,
      visitor2: 200,
      visitorList: [],
      visitorListAll: 0,
      shop1: 100,
      shop2: 200,
      shopList: [],
      shopListAll: 0,
      cafe1: 100,
      cafe2: 200,
      cafeList: [],
      cafeListAll: 0,
      medical1: 100,
      medical2: 200,
      medicalList: [],
      medicalListAll: 0,
      current: 0,
      current2: 0,
      toiletNum: 0,
      toiletAreaLine: 0,
      toiletNumLine: 0,
      index1: 0,
      index2: 0,
      index3: 0,
      index4: 0,
      index5: 0,
      index6: 0,
      index7: 0,
      index8: 0,
      allFacSum: 0,
      lastSum: 0,
      strollRestArea: 0,
      rateNumNew: 0,
      toiletListDetail0: [],
      toiletListDetail1: [],
      toiletListDetail2: [],
      toiletListDetail3: [],
      showToilet: [],
      lastMax: 0
    }
  },
  methods: {
    showHint (flag) {
      if (flag === 1) {
        wx.showToast({
          title: '在该情况下不需要设置该类用地',
          icon: 'none',
          duration: 2000
        })
      } else {
        wx.showToast({
          title: '在该情况下必须要设置该类用地',
          icon: 'none',
          duration: 2000
        })
      }
    },
    showToiletFun (i, status) {
      this.$set(this.showToilet, i, status)
    },
    numFilter (value) {
      let realVal = ''
      if (!isNaN(value) && value !== '') {
        // 截取当前数据到小数点后三位
        let transformVal = Number(value).toFixed(2)
        realVal = transformVal.substring(0, transformVal.length - 1)
      } else {
        realVal = '--'
      }
      console.log('numFilter', Number(realVal))
      return realVal
    },
    submit () {
      this.sdata.hdgNum = this.activityList.length
      this.sdata.hdgArea = this.activityListAll
      this.sdata.zlgNum = this.exhibitionList.length
      this.sdata.zlgArea = this.exhibitionListAll
      this.sdata.tltxNum = this.galleryList.length
      this.sdata.tltxArea = this.galleryListAll
      this.sdata.csNum = this.toiletList.length
      this.sdata.csArea = this.toiletListAll
      this.sdata.csNum = this.toiletList.length
      this.sdata.csdwNum = this.toiletList.length
      this.sdata.ykArea = this.visitorListAll
      this.sdata.ykNum = this.visitorList.length
      this.sdata.xmbArea = this.shopListAll
      this.sdata.xmbNum = this.shopList.length
      this.sdata.cafeArea = this.cafeListAll
      this.sdata.cafeNum = this.cafeList.length
      this.sdata.ylArea = this.medicalListAll
      this.sdata.ylNum = this.medicalList.length
      this.sdata.allFacSum = this.allFacSum
      this.sdata.allFacSumRate = (this.allFacSum * 100 / this.sdata.landArea) | this.numFilter
      wx.setStorageSync('area', this.sdata)
      wx.navigateBack()
    },
    changeSwiper (event) {
      console.log(event.mp.detail)
      this.strollType = event.mp.detail.currentItemId
      this.current = event.mp.detail.current
    },
    changeSwiper2 (event) {
      console.log(event.mp.detail)
      this.strollType2 = event.mp.detail.currentItemId
      this.current2 = event.mp.detail.current
    },
    onChange (index, type, e) {
      if (type === 'huodong') {
        const valueName = 'activity' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'tinglang') {
        const valueName = 'gallery' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'zhanlan') {
        const valueName = 'exhibition' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'cs') {
        const valueName = 'toilet' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'yk') {
        const valueName = 'visitor' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'xmb') {
        const valueName = 'shop' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'cafe') {
        const valueName = 'cafe' + index
        this[valueName] = e.mp.detail
      }
      if (type === 'yl') {
        const valueName = 'medical' + index
        this[valueName] = e.mp.detail
      }
    },
    onChangeList (index, type, event) {
      if (type === 'huodong') {
        this.$set(this.activityList, index, event.mp.detail)
      }
      if (type === 'tinglang') {
        this.$set(this.galleryList, index, event.mp.detail)
      }
      if (type === 'zhanlan') {
        this.$set(this.exhibitionList, index, event.mp.detail)
      }
      if (type === 'cs') {
        this.$set(this.toiletList, index, event.mp.detail)
      }
      if (type === 'yk') {
        this.$set(this.visitorList, index, event.mp.detail)
      }
      if (type === 'xmb') {
        this.$set(this.shopList, index, event.mp.detail)
      }
      if (type === 'cafe') {
        this.$set(this.cafeList, index, event.mp.detail)
      }
      if (type === 'yl') {
        this.$set(this.medicalList, index, event.mp.detail)
      }
      if (type === 'csxq0') {
        this.$set(this.toiletListDetail0, index, event.mp.detail)
      }
      if (type === 'csxq1') {
        this.$set(this.toiletListDetail1, index, event.mp.detail)
      }
      if (type === 'csxq2') {
        this.$set(this.toiletListDetail2, index, event.mp.detail)
      }
      if (type === 'csxq3') {
        this.$set(this.toiletListDetail3, index, event.mp.detail)
      }
    },
    addList (type) {
      if (this.allFacSum >= this.sdata.limitrecreation) {
        wx.showToast({
          title: '剩余用地已经用完，请酌情减少其他类用地用量',
          icon: 'none',
          duration: 2000
        })
        return
      }
      this.lastMax = ~~(this.sdata.limitrecreation - this.allFacSum)
      if (type === 'huodong') {
        this.$set(this.activityList, this.activityList.length, 0)
      }
      if (type === 'tinglang') {
        this.$set(this.galleryList, this.galleryList.length, 0)
      }
      if (type === 'zhanlan') {
        this.$set(this.exhibitionList, this.exhibitionList.length, 0)
      }
      if (type === 'cs') {
        this.$set(this.toiletList, this.toiletList.length, 0)
      }
      if (type === 'yk') {
        this.$set(this.visitorList, this.visitorList.length, 0)
      }
      if (type === 'xmb') {
        this.$set(this.shopList, this.shopList.length, 0)
      }
      if (type === 'cafe') {
        this.$set(this.cafeList, this.cafeList.length, 0)
      }
      if (type === 'yl') {
        this.$set(this.medicalList, this.medicalList.length, 0)
      }
    },
    delList (type) {
      if (type === 'huodong') {
        this.$delete(this.activityList, this.activityList.length - 1)
      }
      if (type === 'tinglang') {
        this.$delete(this.galleryList, this.galleryList.length - 1)
      }
      if (type === 'zhanlan') {
        this.$delete(this.exhibitionList, this.exhibitionList.length - 1)
      }
      if (type === 'cs') {
        this.$delete(this.toiletList, this.toiletList.length - 1)
      }
      if (type === 'yk') {
        this.$delete(this.visitorList, this.visitorList.length - 1)
      }
      if (type === 'xmb') {
        this.$delete(this.shopList, this.shopList.length - 1)
      }
      if (type === 'cafe') {
        this.$delete(this.cafeList, this.cafeList.length - 1)
      }
      if (type === 'yl') {
        this.$delete(this.medicalList, this.medicalList.length - 1)
      }
    }
  },
  watch: {
    allFacSum (val) {
      rateNum = this.numFilter(val * 100 / this.sdata.landArea)
      chart.setOption(getOption())
      this.rateNumNew = rateNum
      this.allFacSum = this.numFilter(val)
      if (val > this.sdata.limitrecreation) {
        wx.showToast({
          title: '剩余用地已经超出上限，请酌情减少其他类用地用量',
          icon: 'none',
          duration: 2000
        })
      }
    },
    activityList (val) {
      this.activityListAll = 0
      for (let i = 0; i < this.activityList.length; i++) {
        this.activityListAll += this.activityList[i]
      }
    },
    activityListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    galleryList (val) {
      this.galleryListAll = 0
      for (let i = 0; i < this.galleryList.length; i++) {
        this.galleryListAll += this.galleryList[i]
      }
    },
    galleryListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    exhibitionList (val) {
      this.exhibitionListAll = 0
      for (let i = 0; i < this.exhibitionList.length; i++) {
        this.exhibitionListAll += this.exhibitionList[i]
      }
    },
    exhibitionListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    visitorList (val) {
      this.visitorListAll = 0
      for (let i = 0; i < this.visitorList.length; i++) {
        this.visitorListAll += this.visitorList[i]
      }
    },
    visitorListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    shopList (val) {
      this.shopListAll = 0
      for (let i = 0; i < this.shopList.length; i++) {
        this.shopListAll += this.shopList[i]
      }
    },
    shopListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    cafeList (val) {
      this.cafeListAll = 0
      for (let i = 0; i < this.cafeList.length; i++) {
        this.cafeListAll += this.cafeList[i]
      }
    },
    cafeListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    medicalList (val) {
      this.medicalListAll = 0
      for (let i = 0; i < this.medicalList.length; i++) {
        this.medicalListAll += this.medicalList[i]
      }
    },
    medicalListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    },
    toiletList (val) {
      this.toiletListAll = 0
      for (let i = 0; i < this.toiletList.length; i++) {
        this.toiletListAll += this.toiletList[i]
      }
      this.toiletNum = ~~((this.toiletListAll - 14 * (2 + this.toiletList.length)) / 6)
      this.toiletNum = this.toiletNum > 0 ? this.toiletNum : 0
    },
    toiletListAll (val) {
      this.allFacSum = this.activityListAll + this.galleryListAll + this.exhibitionListAll + this.visitorListAll + this.shopListAll + this.cafeListAll + this.medicalListAll + this.toiletListAll
      this.lastSum = ~~(this.strollRestArea - this.allFacSum)
    }
  },
  mounted () {
    // let app = getApp()
    // this.sdata = wx.getStorageSync('area')
    console.log(~~(this.sdata.rateNum[2] / 100 * this.sdata.landArea))
    this.greenPer = this.sdata.greenPer.bottom
    this.peopleAbility = this.sdata.landArea / this.greenPer
    this.toiletNumLine = this.sdata.landArea < 100000 ? ~~(this.peopleAbility * 0.02) : ~~(this.peopleAbility * 0.15)
    this.toiletAreaLine = this.toiletNumLine * 14
    this.index = new Array(20).fill(0)
    this.strollRestArea = ~~(this.sdata.rateNum[2] / 100 * this.sdata.landArea)
    this.lastSum = this.strollRestArea
    console.log(this.chart, 111)
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
  font-size: 50px;
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
.green-pro-special {
  padding-left: 5px;
}
.green-pro-special .greenslider {
  width: 130px;
}
.arrow-toilet {
  text-align: center;
  padding: 5px 0;
}
</style>
