<script>
import { RouterLink } from 'vue-router';

import AppHeader from './components/AppHeader.vue'

export default {

  name: 'App',
  components: {
    AppHeader
  }
}


</script>

<template>

  <AppHeader />

  <main>
    <router-view></router-view>
  </main>

</template>

<style lang="scss">

</style>