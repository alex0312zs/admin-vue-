<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted(){
    //设置弹窗 是否需要全屏展示
        if (this._isMobile()) {
            //移动端
            this.$store.state.settings.DialogShowFullScreen = true;
        } else {
            //PC端
            this.$store.state.settings.DialogShowFullScreen = false;
        }
  },
  methods:{
     _isMobile() {
            let flag = navigator.userAgent.match(
                /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
            );
            return flag;
        },
  }
}
</script>
