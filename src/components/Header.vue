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
    <v-btn text>
      <router-link to="/">Home</router-link>
    </v-btn>
    <v-btn text>
      <router-link to="/charts">Charts</router-link>
    </v-btn>
    <v-btn text>
      <router-link to="about">About</router-link>
    </v-btn>
  </v-app-bar>
</template>
<style lang="sass" scoped>
button
  span
    a
      color: white
      text-decoration: none
h2
  color: #ededed
</style>
