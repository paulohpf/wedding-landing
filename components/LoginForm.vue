<template>
  <v-form v-model="loginForm.valid" @submit.prevent="submit">
    <v-container>
      <v-row>
        <v-col>
          <v-text-field
            v-model="loginForm.email"
            label="E-mail"
            :rules="rules.emailRules"
          />
        </v-col>
        <v-col>
          <v-text-field
            v-model="loginForm.password"
            type="password"
            label="Senha"
          />
        </v-col>
        <v-col>
          <v-btn :disabled="!loginForm.valid" @click="doLogin">Entrar</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'LoginForm',
  data() {
    return {
      loginForm: {
        email: '',
        password: '',
        valid: false,
      },
      rules: {
        emailRules: [
          (v) => !!v || 'E-mail é obrigatório',
          (v) => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        password: [(v) => !!v || 'Senha é obrigatória'],
      },
    }
  },
  methods: {
    doLogin() {
      // this.$fire.auth
      //   .signInWithEmailAndPassword(
      //     this.loginForm.email,
      //     this.loginForm.password
      //   )
      //   .catch((err) => {
      //     // eslint-disable-next-line no-console
      //     console.error(err)
      //   })
    },
  },
}
</script>

<style lang="scss" scoped>
</style>
