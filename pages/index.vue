<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-form>
        <v-text-field v-model="inviteName" />

        <v-btn @click="getInvite">Pesquisar convite</v-btn>
      </v-form>

      <v-row>
        <v-col v-for="(guest, key) in inviteGuests" :key="key" cols="12">
          {{ guest }}
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
data: () => ({
    inviteName: 'paulo-e-jussara',
    inviteGuests: [],
  }),
  updated() {
    console.log(this)
  },
  methods: {
    getInvite() {
      this.$fireModule
        .firestore()
        .collection('invitations')
        .doc(this.inviteName)
        .get('guests')
        .then((res) => {
          console.log(res.data())
          this.inviteGuests = res.data().guests;
          console.log(this.inviteGuests);
        })
    },
  },
}
</script>
