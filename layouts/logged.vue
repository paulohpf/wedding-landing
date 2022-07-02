<template>
  <v-app dark>
    <v-main>
      <template v-if="!verifyUser">
        <LoginForm />
      </template>

      <template v-if="verifyUser">
        <Nuxt />
      </template>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'LoggedLayout',
  components: {
    LoginForm: () => import('@/components/LoginForm.vue'),
  },
  computed: {
    verifyUser() {
      return this.$fire.auth.currentUser
    },
  },
}
</script>

<style lang="scss" scoped>
</style>
