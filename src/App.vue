<template>
  <v-app>
    <Header />
    <v-main class="container">
      <v-container fluid>
        <v-layout align-center justify-center fill-height>
          <router-view />
        </v-layout>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Header from './components/Header'
export default {
  name: 'App',
  components: { Header }
}
</script>
<style lang="sass" scoped>
.container
  background: #f3f3f3
</style>
