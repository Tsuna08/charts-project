<template>
  <v-app-bar app color="#5c6bc0">
    <div class="d-flex align-center">
      <v-img
        alt="Charts Logo"
        class="shrink mr-2"
        contain
        :src="require('../assets/logo.png')"
        transition="scale-transition"
        width="60"
      />
      <h2>Charts</h2>
    </div>
    <v-spacer></v-spacer>
    <v-btn text v-for="router in routers" :key="router.name" :to="router.link" color="white">
      {{ router.name }}
    </v-btn>
  </v-app-bar>
</template>
<script>
export default {
  data() {
    return {
      routers: [
        { link: '/', name: 'Home' },
        { link: '/charts', name: 'Charts' },
        { link: '/one-chart', name: 'One chart' }
      ]
    }
  }
}
</script>

<style lang="sass" scoped>
button
  span
    a
      color: white
      text-decoration: none
h2
  color: #ededed
</style>
