<template>
  <div>
    <template v-if="!verifyUser">
      <LoginForm />
    </template>

    <template v-if="verifyUser">
      <Nuxt />
    </template>
  </div>
</template>

<script>
export default {
  name: 'LoggedArea',
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
