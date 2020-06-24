<template>
<div class="park-type-container">
  <div class="park-type">

    <!-- <div class="nav-title">景观小工具</div> -->
    <div class="park-type__area">
      <div class="park-type__area__title">
        <p class="park-type__area__title--white">场地面积</p>
        <p class="park-type__area__title--normal">请填写陆地和水域面积</p>
      </div>
      <div class="park-type__area__input">
        <div class="park-type__area__input__line">
          <img src="/static/images/landarea.png" alt="" class="area-line__logo">
          陆地面积
          <input class="area-line__enter" type="text" placeholder="不可为空值*" v-model="landArea">
          ㎡
        </div>
        <div class="park-type__area__input__line">
          <img src="/static/images/waterarea.png" alt="" class="area-line__logo">
          水域面积
          <input class="area-line__enter" type="text" placeholder="0" v-model="waterArea">
          ㎡
        </div>
      </div>
      <div class="park-type__area__title park-type__area__title--bottom">
        <p class="park-type__area__title--green">公园类型</p>
        <p class="park-type__area__title--normal">请根据陆地面积值有以下可选类型</p>
      </div>
    </div>
  </div>

  <div class="swiper area-swiper">
    <swiper
      class="swiper-inner"
      :indicator-dots='false'
      :autoplay='false'
      :interval='3000'
      :circular='circular'
      :previous-margin="'100px'"
      :next-margin="'100px'"
      @change='changeSwiper'
    >
      <a  v-for='img in topSwipers' :key='img.id'>

        <swiper-item class="swiper-bar">
          <img              
            class='slide-image' 
            mode='aspectFit' 
            :src="img.outterImage" 
            />
        </swiper-item>
      </a>
    </swiper>  
    
  </div> 

  <div class="green-bottom-btn get-report-btn" @click="getReport">获取报告</div>
</div>
  
</template>

<script>
export default {
  data () {
    return {
      topSwipers: [{
        id: 1,
        outterImage: '../../static/images/zoo.png'
      },
      {
        id: 1,
        outterImage: '../../static/images/plant.png'
      },
      {
        id: 1,
        outterImage: '../../static/images/combine.png'
      },
      {
        id: 1,
        outterImage: '../../static/images/park.png'
      },
      {
        id: 1,
        outterImage: '../../static/images/commity.png'
      },
      {
        id: 1,
        outterImage: '../../static/images/other.png'
      }],
      nowType: '0',
      landArea: null,
      waterArea: null
    }
  },
  methods: {
    changeSwiper (event) {
      console.log(event.mp.detail.current)
      this.nowType = event.mp.detail.current
    },
    getReport () {
      console.log(this.waterArea, this.landArea, this.nowType)
      if (!this.waterArea) {
        wx.showToast({
          title: '请输入水域面积',
          icon: 'none',
          duration: 2000
        })
      } else if (!this.landArea) {
        wx.showToast({
          title: '请输入陆地面积',
          icon: 'none',
          duration: 2000
        })
      } else {
        const area = {
          landArea: this.landArea,
          waterArea: this.waterArea,
          parkType: this.nowType
        }
        wx.setStorage({
          key: 'area',
          data: area
        })
        const url = '../overView/main'
        mpvue.navigateTo({ url })
      }
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>
.park-type-container {
  height: 100%;
}
.park-type {
  display: block;
  height: 35%;
  width: 100%;
  background: -webkit-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -o-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -mos-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: -moz-linear-gradient(left bottom,#08B26B,#0FC1A2);
  background: linear-gradient(left bottom,#08B26B,#0FC1A2);
  border-bottom-left-radius: 550px 30px;
  border-bottom-right-radius: 550px 30px;
}
.park-type__area {
  height: 40%;
  text-align: left;
  padding-top: 30%;
  padding-left: 20px;
  padding-right: 20px;
}
.park-type__area__title--white {
  font-size: 28px;
  font-weight: 500;
  color: #FFFFFF;
}
.park-type__area__title--green {
  font-size: 28px;
  font-weight: 500;
  color: #21c3a2;
}
.park-type__area__title--normal {
  padding-top: 10px;
  font-size: 12px;
}
.park-type__area__input {
  background: #FFFFFF;
  border-radius: 5px;
  margin-top: 50px;
  padding: 0 15px;
  box-shadow: whitesmoke 0px 2px 3px;
}
.park-type__area__input__line {
  height: 50px;
  line-height: 50px;
  padding: 15px 0;
}
.park-type__area__input__line:last-child {
  border-top: .5px solid #f0f0f0;
}
.area-line__logo {
  display: inline-block;
  vertical-align: middle;
  width: 25px;
  height: 25px;
}
.area-line__enter{
  display: inline-block;
  vertical-align: middle;
  text-align: right;
}
.park-type__area__title--bottom {
  margin-top: 30px;
}
.area-swiper {
  margin-top: 200px;
  height: 200px;
}
.swiper-inner {
  height: 100%;
}
.swiper-bar {
  text-align: center;
}
.slide-image{
  width:160px;
  height:200px;
  margin: 0 10px;
}
.get-report-btn {
  margin: 15px auto;
}
</style>
