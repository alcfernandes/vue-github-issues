<template>
    <v-container text-xs-center grid-list-md>
      <h1>Experimento Vue.js - Github Issues</h1>
      <p class="body-2">Simples exercício para experimentar o Vue.js com Vuetify.<br>Esse app busca e exibe as issues de um repo do
        GitHub.
        <br>Inspirado nos vídeos do Tiago Matos, "Vuejs do jeito ninja!".
      </p>

      <v-layout row>
        <v-flex xs4>
          <v-text-field
            v-model="username" solo
            label="User name"
            required
            :error-messages="userNameErrors"
            @input="$v.username.$touch()"
            @blur="$v.username.$touch()"
          ></v-text-field>
        </v-flex>

        <v-flex xs4>
          <v-text-field
            v-model="repository" solo
            label="Repository"
            required
            :error-messages="repositoryErrors"
            @input="$v.repository.$touch()"
            @blur="$v.repository.$touch()"
          ></v-text-field>
        </v-flex>

        <v-flex xs4>
          <v-btn @click.prevent.stop="getIssues()" color="success">Go</v-btn>
          <v-btn @click.prevent.stop="fieldsReset()" color="error">Limpar</v-btn>
        </v-flex>
      </v-layout>

      <v-layout row>
        <v-flex xs12>
          <v-data-table
        :headers="headers"
        :items="issues"
        :pagination.sync="pagination"
        class="elevation-1"
        rows-per-page-text="Linhas por página"
      >
        <template slot="items" slot-scope="props">
          <td>{{ props.item.number }}</td>
          <td class="text-xs-left">{{ props.item.title }}</td>
        </template>

        <template slot="no-data">
          <v-alert :value="true" color="info" icon="warning">
            Parece que não há issues nesse repo.
          </v-alert>
        </template>

      </v-data-table>
        </v-flex>
      </v-layout>

    </v-container>

</template>

<script>
import axios from 'axios'
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],

  validations: {
    username: {required},
    repository: {required}
  },

  data: () => ({
    username: '',
    repository: '',
    issues: [],
    headers: [
      {text: 'Número', value: 'number'},
      {text: 'Título', value: 'title'}
    ],
    pagination: {
      sortBy: 'number'
    }
  }),

  methods: {
    fieldsReset () {
      this.$v.$reset()
      this.username = ''
      this.repository = ''
      this.issues = []
    },

    getIssues () {
      this.$v.$touch()

      if (!this.$v.$invalid) {
        const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues`

        axios.get(url)
          .then((response) => {
            this.issues = response.data
          })
          .catch((error) => {
            console.log(error.response.data)
            // this.response.status = 'error'
            // this.response.message = 'Repositório não existe!'
          })
          .finally(() => {
            // this.loader.getIssues = false
          })
      }
    }
  },

  computed: {
    userNameErrors () {
      const errors = []
      if (!this.$v.username.$dirty) return errors
      !this.$v.username.required && errors.push('O nome de usuário é necessário.')
      return errors
    },

    repositoryErrors () {
      const errors = []
      if (!this.$v.repository.$dirty) return errors
      !this.$v.repository.required && errors.push('O repositório é necessário.')
      return errors
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
