<template>
  <div>
    <div class="banner">
      <v-img :src="banner">
        <div
          class="banner-content d-flex flex-column justify-center align-center"
        >
          <div class="names">
            <span class="engaged">Fernanda</span> &
            <span class="engaged">Paulo Henrique</span>
          </div>
          <div class="date">25/11/2022</div>
        </div>
      </v-img>
    </div>
    <v-container class="pt-0 mt-0" fluid>
      <v-row justify="center" align="center">
        <v-col class="py-6" cols="8">
          <h2 class="title text-center">
            Contagem Regressiva para nosso grande dia
          </h2>
        </v-col>
        <v-col class="py-6" cols="8">
          <h2 class="title text-center">
            Confirme a sua presença em nosso casamento!
          </h2>
          <v-form>
            <template v-if="!inviteGuests.length">
              <span>
                Digite o nome conforme consta em seu convite para confirmar sua
                presença.
              </span>

              <v-text-field v-model="inviteName" label="Nome no convite" />
              <v-btn :loading="inviteLoading" @click="getInvite">
                Pesquisar convite
              </v-btn>
            </template>

            <template v-if="inviteGuests.length">
              <span>
                Digite o nome conforme consta em seu convite para confirmar sua
                presença.
              </span>

              <div class="d-flex flex-row justify-center">
                <v-checkbox
                  v-for="(guest, key) in inviteGuests"
                  :key="key"
                  v-model="inviteConfirmGuests"
                  :value="guest"
                  class="pa-2"
                  :label="guest"
                />
              </div>
              <div class="d-flex flex-row justify-center">
                <v-btn :loading="inviteLoading" @click="confirmInvite">
                  Confirmar Presença
                </v-btn>
              </div>
            </template>
          </v-form>

          <!-- Modal de Erro -->
          <v-dialog v-model="inviteFormError">
            <v-card>
              <v-card-title class="text-h5">
                Ops, convite não encontrado!
              </v-card-title>

              <v-card-text>
                Seu convite não foi encontrado, verifique se o nome está escrito
                conforme consta em seu convite e tente novamente.
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn text @click="inviteFormError = false">Entendi</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>

          <v-dialog v-model="inviteFormSuccess">
            <v-card>
              <v-card-title class="text-h5">
                Confirmado com sucesso!
              </v-card-title>

              <v-card-text> Sua presença foi confirmada, caso </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn text @click="inviteFormSuccess = false">Entendi</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-col>
        <v-col class="py-6" cols="8">
          <h2 class="title text-center">
            Nosso pix para caso queira nos presentear com alguma cota para nossa
            lua de mel
          </h2>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'LandingPage',
  data: () => ({
    banner: require('@/assets/images/banner.jpg'),
    inviteFormError: false,
    inviteFormSuccess: false,
    inviteLoading: false,
    inviteName: 'Paulo e Jussara',
    inviteGuests: [],
    inviteConfirmGuests: [],
  }),
  updated() {
    console.log(this)
  },
  methods: {
    resetInvite() {
      this.inviteGuests = []
      this.inviteConfirmGuests = []
      this.inviteFormError = false
      this.inviteFormSuccess = false
    },
    getInvite() {
      this.resetInvite()
      this.inviteLoading = true

      const inviteNameFormatted = this.inviteName
        .replace(/\s/g, '-')
        .toLowerCase()

      this.$fireModule
        .firestore()
        .collection('invitations')
        .doc(inviteNameFormatted)
        .get('guests')
        .then((res) => {
          console.log(res.data())
          this.inviteGuests = res.data().guests
          console.log(this.inviteGuests)

          this.inviteLoading = false
        })
        .catch(() => {
          this.inviteFormError = true
          this.inviteLoading = false
        })
    },
    confirmInvite() {
      this.resetInvite()
      this.inviteFormSuccess = true
    },
  },
}
</script>

<style lang="scss">
.banner {
  .v-image__image {
    opacity: 0.4;
  }

  .banner-content {
    height: 100%;
  }

  .names {
    .engaged {
      color: rgb(20, 93, 160);
    }
  }
}
</style>
