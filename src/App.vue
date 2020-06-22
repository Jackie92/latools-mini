<script>
export default {
  created () {
    console.log('mpvue')
  }
}
</script>

<style>
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
.nav-title {
  text-align: center;
  padding-top: 50px;
  color: white;
  font-family: '微软雅黑';
}
</style>
