<template>
    <v-container text-xs-center grid-list-md>
      <h1>Experimento Vue.js - Github Issues</h1>
      <p class="body-2">Simples exercício para experimentar o Vue.js com Vuetify.<br>Esse app busca e exibe as issues de um repo do
        GitHub.
        <br>Inspirado nos vídeos do Tiago Matos, "Vuejs do jeito ninja!".
      </p>

      <v-layout row>
        <v-flex xs4>
          <v-text-field v-model="username" solo label="User name"></v-text-field>
        </v-flex>

        <v-flex xs4>
          <v-text-field v-model="repository" solo label="Repository"></v-text-field>
        </v-flex>

        <v-flex xs4>
          <v-btn color="success">Go</v-btn>
          <v-btn color="error">Limpar</v-btn>
        </v-flex>
      </v-layout>

    </v-container>
</template>

<script>
export default {
  data: () => ({
    username: '',
    repository: ''
  })
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
