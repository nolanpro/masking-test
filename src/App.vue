<template>
  <div id="app">
    <mask-test />
  </div>
</template>

<script>
import Vue from 'vue'
import MaskTest from './components/MaskTest.vue'
import BootstrapVue from 'bootstrap-vue'

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

Vue.use(BootstrapVue)

export default {
  name: 'app',
  components: {
    MaskTest
  }
}
</script>

<style>
#app {
}
</style>
