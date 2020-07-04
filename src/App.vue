<script>
export default {
  created () {
    console.log('mpvue')
    // wx.setStorageSync('area', {})
  }
}
</script>

<style>
/* 公共样式 */
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 200rpx 0;
  box-sizing: border-box;
  font-family: '微软雅黑';
}
/* this rule will be remove */
* {
  transition: width 2s;
  -moz-transition: width 2s;
  -webkit-transition: width 2s;
  -o-transition: width 2s;
  font-family: '微软雅黑';
}
page {
  width: 100%;
  height: 100%;
  font-family: '微软雅黑';
}
.fr {
  float: right;
}
.fl {
  float: left;
}
.chart-edit {
  width: 20px;
  height: 20px;
  float: right;
}
.icon-wrapper {
  margin-top: 15px;
}
.icon-line {
  margin: 11px 0;
  background: #FFFFFF;
  border-radius: 10px;
  padding: 15px;
}
.icon-line--right {
  border-radius: 0 0 10px 10px;
  margin: 0 0 11px 0;
}
.icon-line--title--line {
  vertical-align: middle;
  height: 27px;
  line-height: 27px;
}
.icon-line--title--icon {
  width: 25px;
  height: 25px;
}
.icon-line--title--text {
  position: relative;
  top: -6px;
  left: 10px;
}
.icon-line--body--line {
  padding-top: 15px;
}
.icon-wrapper .icon-line--body--line {
  padding-top: 8px;
}
.icon-web-line {
  margin: 30px 0;
  overflow: hidden;
}
.icon-describe {
  margin-top: 10px;
}
.icon-content {
  float: left;
  width: 25%;
  text-align: center;
}
.green-title {
  border-left: #0FC1A2 solid 5px ;
  padding-left: 5px;
  font-size: 17px;
  color: #333333;
  margin-bottom: 10px;
  font-weight: bold;
}
.green-spread {
  border-top: #D2D2D2 solid .5px;
  text-align: center;
  margin-top: 30px;
}
.green-spread-inner {
  background: #FFFFFF;
  padding: 0 5px;
  position: relative;
  top: -10px;
  color: #0FBFA3;
  font-size: 16px;
}
/* 公共组件样式 */
.nav-title {
  /* 顶部标题 */
  text-align: center;
  padding-top: 50px;
  color: white;
  font-family: '微软雅黑';
}
.white-bottom-btn {
  /* 底部大按钮，白底绿字 */
  width: 90%;
  background: #FFFFFF;
  color: #13987E;
  text-align: center;
  border-radius: 25px;
  height: 40px;
  line-height: 40px;
  font-weight: 600;
}
.green-bottom-btn {
  /* 底部大按钮，绿地白字 */
  width: 90%;
  background: #21c3a2;
  color: #FFFFFF;
  text-align: center;
  border-radius: 25px;
  height: 40px;
  line-height: 40px;
  font-weight: 600;
}
</style>
