<template>
  <div>
    <div class="banner font-buffaloscript">
      <v-img :src="banner">
        <div
          class="banner-content d-flex flex-column justify-center align-center"
        >
          <div class="names">
            <span class="engaged">Fernanda</span> &
            <span class="engaged">Paulo Henrique</span>
          </div>
          <div class="date">25 / Novembro / 2022</div>
        </div>
      </v-img>
    </div>
    <v-container fluid>
      <v-row justify="center" align="center">
        <v-col class="section py-6 d-flex justify-center" cols="12">
          <v-container class="pa-0 ma-0">
            <h2 class="title text-center">
              Contagem Regressiva para nosso grande dia
            </h2>

            <CountDown :deadline="new Date(2022, 10, 25, 18)" />

            <p>
              Olá queridos amigos e familiares. A contagem regressiva começa, o
              frio na barriga e toda a ansiedade do dia mais esperado de nossas
              vidas, e nos enche de alegria em tê-los ao nosso lado! Vamos
              juntos nesse grande sonho, o dia em que uniremos nossas almas e
              corpos para sempre, o dia do nosso casamento!!!
            </p>
          </v-container>
        </v-col>
        <v-col class="section py-6 d-flex justify-center" cols="12">
          <v-container>
            <h2 class="title text-center">Confirmação de presença</h2>

            <p>Confirme a sua presença em nosso casamento!</p>
            <p>
              <a
                href="https://api.whatsapp.com/send/?phone=5514988375191&text=Ol%C3%A1%2C+gostaria+de+confirmar+minha+presen%C3%A7a+no+casamento+da+Fer+e+do+Ph%21+%3A%5D&type=phone_number&app_absent=0"
                target="_blank"
                rel="noopener noreferrer"
                >Clique aqui</a
              >
              e você será direcionado ao número do WhatsApp, envie uma mensagem
              confirmando sua presença e de quem foi convidado com você (ver
              convite individual).
            </p>
          </v-container>
        </v-col>
        <v-col class="section py-6 d-flex justify-center" cols="12">
          <v-container class="pa-0 ma-0">
            <h2 class="title text-center">Lista de Presentes</h2>

            <span class="d-block text-center"
              >Veja nossa lista de presentes
              <a
                href="https://www.querodecasamento.com.br/lista-de-casamento/fernanda-santos-paulo-henrique"
                target="_blank"
                rel="noopener noreferrer"
                >clicando aqui</a
              >.</span
            >
          </v-container>
        </v-col>
        <v-col class="section pix-wedding py-6 d-flex justify-center" cols="12">
          <v-container class="pa-0 ma-0">
            <h2 class="title text-center">
              Nosso pix para caso queira nos presentear com alguma cota para
              nossa lua de mel
            </h2>

            <v-row>
              <v-col cols="12" class="d-flex justify-center">
                <v-btn :disabled="pix.visible === 0" @click="setVisiblePix(0)"
                  >R$50</v-btn
                >
                <v-btn :disabled="pix.visible === 1" @click="setVisiblePix(1)"
                  >R$100</v-btn
                >
                <v-btn :disabled="pix.visible === 2" @click="setVisiblePix(2)"
                  >Livre</v-btn
                >
              </v-col>
              <v-col
                cols="12"
                class="d-flex flex-column align-center text-center"
              >
                <v-img
                  max-width="250px"
                  :src="pix.pixes[pix.visible].img"
                  contain
                  @click="copyPix"
                />
                <span>Clique na imagem para copiar</span>
                <v-snackbar v-model="pix.snackbar" timeout="3000">
                  Pix copiado para a área de transferencia

                  <template #action="{ attrs }">
                    <v-btn
                      color="pink"
                      text
                      v-bind="attrs"
                      @click="pix.snackbar = false"
                    >
                      Close
                    </v-btn>
                  </template>
                </v-snackbar>
              </v-col>
            </v-row>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'LandingPage',
  components: {
    CountDown: () => import('@/components/CountDown.vue'),
  },
  layout(context) {
    // if (context.$fire.auth.currentUser) {
    //   return 'logged'
    // } else {
    //   return 'default'
    // }
    return 'default'
  },
  data: () => ({
    banner: require('@/assets/images/banner.jpg'),
    pix: {
      visible: 0,
      snackbar: false,
      pixes: [
        {
          code: '00020126580014BR.GOV.BCB.PIX01366bee65ed-4328-4436-822c-917fa60b0ba2520400005303986540550.005802BR5924Paulo Henrique P. Franco6005Bauru62070503***63047A4D',
          img: require('@/assets/images/pix/pix50.png'),
        },
        {
          code: '00020126580014BR.GOV.BCB.PIX01366bee65ed-4328-4436-822c-917fa60b0ba25204000053039865406100.005802BR5924Paulo Henrique P. Franco6005Bauru62070503***63041031',
          img: require('@/assets/images/pix/pix100.png'),
        },
        {
          code: '00020126580014BR.GOV.BCB.PIX01366bee65ed-4328-4436-822c-917fa60b0ba25204000053039865802BR5924Paulo Henrique P. Franco6005Bauru62070503***63040548',
          img: require('@/assets/images/pix/free.png'),
        },
      ],
    },
    inviteFormError: false,
    inviteFormSuccess: false,
    inviteLoading: false,
    inviteName: '',
    inviteGuests: [],
  }),
  computed: {
    inviteNameFormatted() {
      return String(this.inviteName)
        .normalize('NFD')
        .replace(/[\u0300-\u036F]/g, '')
        .toLowerCase()
    },
  },
  mounted() {
    /**
     * Cloudflare Web Analytics
     */
    const script = document.createElement('script')
    script.onload = this.onScriptLoaded
    script.setAttribute('defer', true)
    script.setAttribute(
      'data-cf-beacon',
      '{"token": "159f7f956c944ee1b9b1664bbc30a1a9"}'
    )
    script.type = 'text/javascript'
    script.src = 'https://static.cloudflareinsights.com/beacon.min.js'
    document.head.appendChild(script)
  },
  methods: {
    resetInvite() {
      this.inviteGuests = []
      this.inviteFormError = false
      this.inviteFormSuccess = false
    },
    getInvite() {
      // this.resetInvite()
      // this.inviteLoading = true
      // this.$fireModule
      //   .firestore()
      //   .collection('invitations')
      //   .doc(this.inviteNameFormatted)
      //   .get('guests')
      //   .then((res) => {
      //     this.inviteGuests = res.data().guests
      //     this.inviteLoading = false
      //   })
      //   .catch(() => {
      //     this.inviteFormError = true
      //     this.inviteLoading = false
      //   })
    },
    async confirmInvite() {
      // const invitation = await this.$fireModule
      //   .firestore()
      //   .collection('invitations')
      //   .doc(this.inviteNameFormatted)
      // invitation.update({ guests: this.inviteGuests })
      // this.resetInvite()
      // this.inviteFormSuccess = true
    },
    setVisiblePix(pos) {
      this.pix.visible = pos
    },
    copyPix() {
      navigator.clipboard.writeText(this.pix.pixes[this.pix.visible].code)
      this.pix.snackbar = true
    },
  },
}
</script>

<style lang="scss">
.banner {
  font-size: 3rem;

  .date {
    font-size: 2rem;
  }

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

.title {
  font-weight: 600 !important;
}

.section {
  background: rgb(234, 248, 254);

  &:nth-child(even) {
    background: #fff;
  }

  &:not(:first-child) {
    border-top: 1px solid #b8e7fb;
  }
}
</style>
