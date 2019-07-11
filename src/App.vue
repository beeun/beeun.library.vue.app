<template>
  <SlideBack id="app">
  	<keep-alive>
      <router-view class="appView"></router-view>
    </keep-alive>
  </SlideBack>
</template>

<script>
import SlideBack from './components/SlideBack.vue';
export default {
  name: 'App',
  components: {
  	SlideBack
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
