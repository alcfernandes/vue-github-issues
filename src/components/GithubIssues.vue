<template>
  <v-container text-xs-center grid-list-md>
    <h1>Experimento Vue.js - Github Issues</h1>
    <p class="body-2">Simples exercício para experimentar o Vue.js com Vuetify.<br>Esse app busca e exibe as issues de
      um repo do
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
            <tr @click="showDetail(props.item.number)">
              <td>{{ props.item.number }}</td>
              <td class="text-xs-left">{{ props.item.title }}</td>
            </tr>
          </template>

          <template slot="no-data">
            <v-alert :value="true" color="info" icon="warning">
              Parece que não há issues nesse repo.
            </v-alert>
          </template>

        </v-data-table>
      </v-flex>
    </v-layout>

    <v-snackbar
      color="error"
      v-model="alertaErro"
      :timeout="12000"
    >
      Houve um problema na conexão com o GitHub. Verifique se o usuário e repositório estão corretos.
      <v-btn dark flat @click.native="alertaErro = false">Fechar</v-btn>
    </v-snackbar>

    <template>
      <v-layout row justify-center>
        <v-dialog v-model="detailDialog" persistent width="600px">
          <v-card>
            <v-card-title class="headline">{{this.issue.title}}</v-card-title>
            <v-card-text>{{this.issue.body}}
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green darken-1" flat @click.native="detailDialog = false">fechar</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-layout>
    </template>
  </v-container>

</template>

<script>
import axios from 'axios'
import {validationMixin} from 'vuelidate'
import {required} from 'vuelidate/lib/validators'

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
    issue: {},
    headers: [
      {text: 'Número', value: 'number'},
      {text: 'Título', value: 'title'}
    ],
    pagination: {
      sortBy: 'number'
    },
    alertaErro: false,
    detailDialog: false
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
            this.alertaErro = true
            // this.response.status = 'error'
            // this.response.message = 'Repositório não existe!'
          })
          .finally(() => {
            // this.loader.getIssues = false
          })
      }
    },

    showDetail (number) {
      const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues/${number}`

      axios.get(url)
        .then((response) => {
          this.issue = response.data
          this.detailDialog = true
        })
        .catch((error) => {
          console.log(error.response.data)
          this.alertaErro = true
          // this.response.status = 'error'
          // this.response.message = 'Repositório não existe!'
        })
        .finally(() => {
          // this.loader.getIssues = false
        })
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
