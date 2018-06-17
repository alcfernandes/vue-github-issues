<template>
  <v-app id="inspire">
    <v-toolbar dark color="teal lighten-3">
      <v-toolbar-side-icon></v-toolbar-side-icon>
      <v-toolbar-title class="white--text">GitHub Issues</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <v-container>
        <v-layout>

          <router-view></router-view>

        </v-layout>
      </v-container>
    </v-content>

    <v-footer fixed class="pa-3">
      <v-spacer></v-spacer>
      <div>&copy; {{ new Date().getFullYear() }}</div>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
