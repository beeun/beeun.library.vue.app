<template>
  <div id="app">
  	<SlideBack>
	  	<keep-alive>
	      <router-view class="appView"></router-view>
	    </keep-alive>
	  </SlideBack>
	  <van-tabbar v-model="active">
		  <van-tabbar-item icon="home-o">首页</van-tabbar-item>
		  <van-tabbar-item icon="apps-o">分类</van-tabbar-item>
		  <van-tabbar-item icon="shopping-cart-o" info="5">购物车</van-tabbar-item>
		  <van-tabbar-item icon="manager-o">我的</van-tabbar-item>
		</van-tabbar>
  </div>
</template>

<script>
import SlideBack from './components/SlideBack.vue';
export default {
  name: 'App',
  components: {
  	SlideBack
  },
  data() {
  	return {
  		active: 0
  	}
  }
}
</script>

<style>
body {
	margin: 0;
	padding: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
}
</style>
