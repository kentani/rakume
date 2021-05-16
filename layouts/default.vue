<template>
  <v-app style="background: #F5F5F5">
    <v-app-bar app flat dense color="green lighten-2">
      <v-toolbar-title>
        <span class="title font-weight-bold white--text">RAKUME</span><span class="body-2 font-weight-bold white--text"> 〜 ラクミー 〜</span>
      </v-toolbar-title>
    </v-app-bar>
    <v-main class="my-4">
      <v-container fluid class="my-4">
        <nuxt />
      </v-container>
    </v-main>
    <v-footer app absolute>
      <span>&copy; {{ new Date().getFullYear() }}~</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      title: 'RAKUME - ラクミー -'
    }
  }
}
</script>
