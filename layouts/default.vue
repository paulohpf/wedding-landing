<template>
  <v-app dark>
    <v-main>
      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  computed: {
    verifyUser() {
      // return this.$fire.auth.currentUser
      return null
    },
  }
}
</script>
