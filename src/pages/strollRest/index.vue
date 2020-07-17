<template>
<div class="gl">
  <div class="gl-head">
    <div class="fl">
      <div class="gl-head__title">游憩服务用地</div>
      <div class="gl-head__number">{{sdata.rateNum[2]}}<span class="m2">%</span> </div>
      <div class="gl-head__area-line">
        <div class="area-line__land-area">
          <p>{{~~(sdata.rateNum[2] / 100 * sdata.landArea)}}㎡</p>
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
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">活动馆用地
              </div>
              <div class="bet-item-right">{{2 + activityList.length}}个</div>
            </div>
            <div class="bet-item-be">{{activityListAll + activity1 + activity2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <!-- <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>活动馆1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="150"
                @change="onChange(1, 'huodong', $event)"
                :min='0'
                :max='150'
              />
              <div class="green-pro-left">{{activity1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>活动馆2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="150"
                @change="onChange(2, 'huodong', $event)"
                :min='0'
                :max='150'
              />
              <div class="green-pro-left">{{activity2}}㎡</div>
            </div>
          </div> -->
          <div v-if="activityList.length > 0" v-for="(item, i) in activityList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>活动馆{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="150"
                  @change="onChangeList(i, 'huodong', $event)"
                  :min='0'
                  :max='150'
                />
                <div class="green-pro-left">{{activityList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('huodong')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 游憩类-活动馆end -->
      
      <!-- 游憩类-廊亭厅榭start -->
      <div v-if="strollType === 'tinglang'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">亭廊厅榭用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{galleryListAll + gallery1 + gallery2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>亭廊厅榭1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="20"
                @change="onChange(1, 'tinglang', $event)"
                :min='0'
                :max='20'
              />
              <div class="green-pro-left">{{gallery1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>亭廊厅榭2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="20"
                @change="onChange(2, 'tinglang', $event)"
                :min='0'
                :max='20'
              />
              <div class="green-pro-left">{{gallery2}}㎡</div>
            </div>
          </div>
          <div v-if="galleryList.length > 0" v-for="(item, i) in galleryList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>亭廊厅榭{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="20"
                  @change="onChangeList(i, 'tinglang', $event)"
                  :min='0'
                  :max='20'
                />
                <div class="green-pro-left">{{galleryList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('tinglang')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 游憩类-廊亭厅榭end -->

      <!-- 游憩类-展览馆start -->
      <div v-if="strollType === 'zhanlan'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">展览馆用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{exhibitionListAll + exhibition1 + exhibition2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>展览馆1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="200"
                @change="onChange(1, 'zhanlan', $event)"
                :min='0'
                :max='200'
              />
              <div class="green-pro-left">{{exhibition1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>展览馆2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="300"
                @change="onChange(2, 'zhanlan', $event)"
                :min='0'
                :max='300'
              />
              <div class="green-pro-left">{{exhibition2}}㎡</div>
            </div>
          </div>
          <div v-if="exhibitionList.length > 0" v-for="(item, i) in exhibitionList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>展览馆{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="300"
                  @change="onChangeList(i, 'zhanlan', $event)"
                  :min='0'
                  :max='300'
                />
                <div class="green-pro-left">{{exhibitionList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('zhanlan')"><div class="number">+</div>添加类目</div>
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
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">厕所用地
              </div>
              <div class="bet-item-right">{{2 + toiletList.length}}个</div>
            </div>
            <div class="bet-item-be">{{toiletListAll + toilet1 + toilet2}}㎡</div>
            <div class="green-pro-one">
              <van-progress
                class="van-progress"
                pivot-text="√"
                color="#5380FF"
                show-pivot
                :percentage="25"
              />
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">蹲位数量
              </div>
            </div>
            <div class="green-pro-one">
              <!-- <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress> -->
              <van-progress
                class="van-progress"
                :pivot-text="toiletNum"
                color="#5380FF"
                show-pivot
                :percentage="25"
              />
              <div class="green-pro-left">{{toiletNum}}个</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>厕所1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="150"
                @change="onChange(1, 'cs', $event)"
                :min='0'
                :max='150'
              />
              <div class="green-pro-left">{{toilet1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>厕所2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="150"
                @change="onChange(2, 'cs', $event)"
                :min='0'
                :max='150'
              />
              <div class="green-pro-left">{{toilet2}}㎡</div>
            </div>
          </div>
          <div v-if="toiletList.length > 0" v-for="(item, i) in toiletList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>厕所{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="150"
                  @change="onChangeList(i, 'cs', $event)"
                  :min='0'
                  :max='150'
                />
                <div class="green-pro-left">{{toiletList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('cs')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 建筑类-厕所end -->
      
      <!-- 建筑类-游客服务中心start -->
      <div v-if="strollType2 === 'yk'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">游客服务中心用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{visitorListAll + visitor1 + visitor2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>游客服务中心1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="100"
                @change="onChange(1, 'yk', $event)"
                :min='0'
                :max='100'
              />
              <div class="green-pro-left">{{visitor1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>游客服务中心2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="200"
                @change="onChange(2, 'yk', $event)"
                :min='0'
                :max='200'
              />
              <div class="green-pro-left">{{visitor2}}㎡</div>
            </div>
          </div>
          <div v-if="visitorList.length > 0" v-for="(item, i) in visitorList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>游客服务中心{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="200"
                  @change="onChangeList(i, 'yk', $event)"
                  :min='0'
                  :max='200'
                />
                <div class="green-pro-left">{{visitorList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('yk')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 建筑类-游客服务中心end -->

      <!-- 建筑类-茶座和咖啡start -->
      <div v-if="strollType2 === 'cz'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">茶座/咖啡厅用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{cafeListAll + cafe1 + cafe2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>茶座/咖啡厅1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="100"
                @change="onChange(1, 'cafe', $event)"
                :min='0'
                :max='100'
              />
              <div class="green-pro-left">{{cafe1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>茶座/咖啡厅2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="200"
                @change="onChange(2, 'cafe', $event)"
                :min='0'
                :max='200'
              />
              <div class="green-pro-left">{{cafe2}}㎡</div>
            </div>
          </div>
          <div v-if="cafeList.length > 0" v-for="(item, i) in cafeList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>茶座/咖啡厅{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="200"
                  @change="onChangeList(i, 'cafe', $event)"
                  :min='0'
                  :max='200'
                />
                <div class="green-pro-left">{{cafeList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('cafe')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 建筑类-茶座和咖啡end -->

      <!-- 建筑类-医疗救助站start -->
      <div v-if="strollType2 === 'yl'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">医疗救助站用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{medicalListAll + medical1 + medical2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>医疗救助站1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="100"
                @change="onChange(1, 'yl', $event)"
                :min='0'
                :max='100'
              />
              <div class="green-pro-left">{{medical1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>医疗救助站2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="200"
                @change="onChange(2, 'yl', $event)"
                :min='0'
                :max='200'
              />
              <div class="green-pro-left">{{medical2}}㎡</div>
            </div>
          </div>
          <div v-if="medicalList.length > 0" v-for="(item, i) in medicalList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>医疗救助站{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="200"
                  @change="onChangeList(i, 'yl', $event)"
                  :min='0'
                  :max='200'
                />
                <div class="green-pro-left">{{medicalList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('yl')"><div class="number">+</div>添加类目</div>
        </div>
      </div>
      <!-- 建筑类-医疗救助站end -->

      
      <!-- 建筑类-小卖部start -->
      <div v-if="strollType2 === 'xmb'"  class="swiper-per-body">
        <div class="gl-body-head">
          <div class="three-switch" v-if="sdata.landArea < 200000">
            <div class="switch-item current">off</div>
            <div class="switch-item">on</div>
            <div class="switch-item">save</div>
          </div>
          <div class="three-switch" v-else>
            <div class="switch-item" @click="index=0" :class="index==0 ? 'current':''">off</div>
            <div class="switch-item" @click="index=1" :class="index==1 ? 'current':''">on</div>
            <div class="switch-item" @click="index=2" :class="index==2 ? 'current':''">save</div>
          </div>
        </div>
        <div class="body-between">
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <img class="greenarea" src="/static/icon/hd.png" alt="">小卖部用地
              </div>
              <div class="bet-item-right">2个</div>
            </div>
            <div class="bet-item-be">{{shopListAll + shop1 + shop2}}㎡</div>
            <div class="green-pro-one">
              <progress class="greenpro" percent="79" color="#5380FF" border-radius="5" stroke-width="4"></progress>
              <div class="green-pro-left">79%</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">1</div>小卖部1
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="100"
                @change="onChange(1, 'xmb', $event)"
                :min='0'
                :max='100'
              />
              <div class="green-pro-left">{{shop1}}㎡</div>
            </div>
          </div>
          <div class="bet-item">
            <div class="bet-item-title">
              <div class="bet-item-left">
                <div class="number">2</div>小卖部2
              </div>
            </div>
            <div class="green-pro">
              <van-slider
                class="greenslider"
                bar-height="3.1px"
                active-color="#5380FF"
                inactive-color="#EBEBEB"
                value="200"
                @change="onChange(2, 'xmb', $event)"
                :min='0'
                :max='200'
              />
              <div class="green-pro-left">{{shop2}}㎡</div>
            </div>
          </div>
          <div v-if="shopList.length > 0" v-for="(item, i) in shopList" :key="i">
            <div class="bet-item">
              <div class="bet-item-title">
                <div class="bet-item-left">
                  <div class="number">{{i + 3}}</div>小卖部{{i + 3}}
                </div>
              </div>
              <div class="green-pro">
                <van-slider
                  class="greenslider"
                  bar-height="3.1px"
                  active-color="#5380FF"
                  inactive-color="#EBEBEB"
                  value="200"
                  @change="onChangeList(i, 'xmb', $event)"
                  :min='0'
                  :max='200'
                />
                <div class="green-pro-left">{{shopList[i]}}㎡</div>
              </div>
            </div>
          </div>
          <div class="add" @click="addList('xmb')"><div class="number">+</div>添加类目</div>
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
      topSwipers: [{
        id: 'huodong',
        outterImage: '../../static/images/hdg.png',
        outterImageCur: '../../static/images/hdg2.png'
      },
      {
        id: 'zhanlan',
        outterImage: '../../static/images/zlg.png',
        outterImageCur: '../../static/images/zlg2.png'
      },
      {
        id: 'tinglang',
        outterImage: '../../static/images/tltx.png',
        outterImageCur: '../../static/images/tltx2.png'
      }],
      topSwipers2: [{
        id: 'cs',
        outterImage: '../../static/images/cs.png',
        outterImageCur: '../../static/images/cs2.png'
      },
      {
        id: 'yk',
        outterImage: '../../static/images/yk.png',
        outterImageCur: '../../static/images/yk2.png'
      },
      {
        id: 'xmb',
        outterImage: '../../static/images/xmb.png',
        outterImageCur: '../../static/images/xmb2.png'
      },
      {
        id: 'cz',
        outterImage: '../../static/images/cz.png',
        outterImageCur: '../../static/images/cz2.png'
      },
      {
        id: 'yl',
        outterImage: '../../static/images/yl.png',
        outterImageCur: '../../static/images/yl2.png'
      }],
      sdata: {},
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
      toiletNum: ~~((150 * 2 - 14 * 2) / 6)
    }
  },
  methods: {
    submit () {
      // // this.sdata.greenPerChose = this.greenPer
      // this.sdata.peopleAbility.chose = this.peopleAbility
      // this.sdata.peopleAbility.isChose = true
      // wx.setStorageSync('area', this.sdata)
      // // const url = '../greenLand/main'
      // // mpvue.navigateBack({ url })
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
    },
    addList (type) {
      if (type === 'huodong') {
        this.$set(this.activityList, this.activityList.length, 150)
      }
      if (type === 'tinglang') {
        this.$set(this.galleryList, this.galleryList.length, 20)
      }
      if (type === 'zhanlan') {
        this.$set(this.exhibitionList, this.exhibitionList.length, 300)
      }
      if (type === 'cs') {
        this.$set(this.toiletList, this.toiletList.length, 150)
      }
      if (type === 'yk') {
        this.$set(this.visitorList, this.visitorList.length, 200)
      }
      if (type === 'xmb') {
        this.$set(this.shopList, this.shopList.length, 200)
      }
      if (type === 'cafe') {
        this.$set(this.cafeList, this.cafeList.length, 200)
      }
      if (type === 'yl') {
        this.$set(this.medicalList, this.medicalList.length, 200)
      }
    }
  },
  watch: {
    activityList (val) {
      this.activityListAll = 0
      for (let i = 0; i < this.activityList.length; i++) {
        this.activityListAll += this.activityList[i]
      }
    },
    galleryList (val) {
      this.galleryListAll = 0
      for (let i = 0; i < this.galleryList.length; i++) {
        this.galleryListAll += this.galleryList[i]
      }
    },
    exhibitionList (val) {
      this.exhibitionListAll = 0
      for (let i = 0; i < this.exhibitionList.length; i++) {
        this.exhibitionListAll += this.exhibitionList[i]
      }
    },
    visitorList (val) {
      this.visitorListAll = 0
      for (let i = 0; i < this.visitorList.length; i++) {
        this.visitorListAll += this.visitorList[i]
      }
    },
    shopList (val) {
      this.shopListAll = 0
      for (let i = 0; i < this.shopList.length; i++) {
        this.shopListAll += this.shopList[i]
      }
    },
    cafeList (val) {
      this.cafeListAll = 0
      for (let i = 0; i < this.cafeList.length; i++) {
        this.cafeListAll += this.cafeList[i]
      }
    },
    medicalList (val) {
      this.medicalListAll = 0
      for (let i = 0; i < this.medicalList.length; i++) {
        this.medicalListAll += this.medicalList[i]
      }
    },
    toiletList (val) {
      this.toiletListAll = 0
      for (let i = 0; i < this.toiletList.length; i++) {
        this.toiletListAll += this.toiletList[i]
      }
      this.toiletNum = ~~((this.toiletListAll + this.toilet1 + this.toilet2 - 14 * (2 + this.toiletList.length)) / 6)
    },
    toilet1 (val) {
      this.toiletNum = ~~((this.toiletListAll + this.toilet1 + this.toilet2 - 14 * (2 + this.toiletList.length)) / 6)
    },
    toilet2 (val) {
      this.toiletNum = ~~((this.toiletListAll + this.toilet1 + this.toilet2 - 14 * (2 + this.toiletList.length)) / 6)
    }
  },
  mounted () {
    // let app = getApp()
    this.sdata = wx.getStorageSync('area')
    this.greenPer = this.sdata.greenPer.bottom
    this.peopleAbility = this.sdata.landArea / this.greenPer
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
</style>
