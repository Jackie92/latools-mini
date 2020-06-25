<script>
export default {
  created () {
    console.log('mpvue')
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
