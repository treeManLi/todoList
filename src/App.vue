<template>
  <div id="app">
  <Header />
  <MainBox />
  <Footer />
</div>
</template>

<script setup>
import Header from '@/components/Header.vue';
import MainBox from './components/MainBox.vue';
import Footer from './components/Footer.vue'
</script>

<style scoped>
#app {
  height: 750px; /* 设置页面高度 */
}
</style>
