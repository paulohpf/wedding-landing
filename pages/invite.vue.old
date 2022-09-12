<template>
  <v-container>
    <v-data-table
      :headers="table.headers"
      :items="invites"
      :items-per-page="10"
      class="elevation-1"
    >
      <template #top>
        <v-toolbar>
          <v-toolbar-title>Gerenciar Convites</v-toolbar-title>
          <v-divider class="mx-4" inset vertical />
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template #activator="{ on, attrs }">
              <v-btn color="primary" class="mb-2" v-bind="attrs" v-on="on">
                Novo convite
              </v-btn>
            </template>
            <v-card>
              <v-form v-model="formValid" @submit.prevent="submit">
                <v-card-title>
                  <span class="text-h5">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          v-model="editedItem.inviteName"
                          :value="editedItem.inviteName"
                          label="Nome no convite"
                          :disabled="editedIndex !== -1"
                        />
                      </v-col>
                      <v-col
                        v-for="(guest, key) in editedItem.guests"
                        :key="key + 1"
                        cols="12"
                      >
                        <v-row>
                          <v-col cols="5">
                            <v-text-field
                              v-model="editedItem.guests[key].name"
                              :value="editedItem.guests[key].name"
                              label="Convidado"
                              :rules="rules.guestName"
                            />
                          </v-col>
                          <v-col cols="5">
                            <v-checkbox
                              v-model="editedItem.guests[key].confirm"
                              class="pa-2"
                              label="Confirmado"
                              :value="editedItem.guests[key].confirm"
                            />
                          </v-col>
                          <v-col cols="2" class="d-flex">
                            <v-icon @click="removeGuest(key)"
                              >mdi-delete</v-icon
                            >

                            <v-icon
                              v-if="editedItem.guests.length === key + 1"
                              @click="addNewGuest"
                              >mdi-plus</v-icon
                            >
                          </v-col>
                        </v-row>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    Cancelar
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    :disabled="!formValid"
                    @click="save"
                  >
                    Salvar
                  </v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-dialog>

          <!-- Delete Dialog -->
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Tem certeza que deseja deletar?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="red darken-1"
                  class="white--text"
                  @click="close"
                  >Cancelar</v-btn
                >
                <v-btn
                  color="green darken-1"
                  class="white--text"
                  @click="deleteItemConfirm()"
                  >Confirmar</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template #item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template #no-data>
        <v-btn color="primary"> Reset </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  name: 'LoggedArea',
  layout: 'logged',
  data() {
    return {
      table: {
        headers: [
          {
            text: 'Convite',
            align: 'start',
            sortable: false,
            value: 'id',
          },
          { text: 'Actions', value: 'actions', sortable: false },
        ],
      },
      rules: {
        guestName: [(v) => !!v || 'Campo Obrigatório'],
      },
      formValid: false,
      invites: [],
      editedIndex: -1,
      editedItem: {
        inviteName: '',
        guests: [
          {
            name: '',
            confirm: false,
          },
        ],
        firebaseInvite: {},
      },
      defaultItem: {
        inviteName: '',
        guests: [
          {
            name: '',
            confirm: false,
          },
        ],
        firebaseInvite: {},
      },
      dialog: false,
      dialogDelete: false,
    }
  },
  computed: {
    verifyUser() {
      return this.$fire.auth.currentUser
    },
    formTitle() {
      return this.editedIndex === -1 ? 'Novo convite' : 'Editar convite'
    },
    inviteNameFormatted() {
      return String(this.editedItem.inviteName).normalize('NFD').replace(/[\u0300-\u036F]/g, "").toLowerCase()
    }
  },
  mounted() {
    this.getInvites()
  },
  methods: {
    getInvites() {
      // this.$fireModule
      //   .firestore()
      //   .collection('invitations')
      //   .get()
      //   .then((res) => {
      //     this.invites = res.docs
      //   })
    },
    getInvite(docId) {
      // return this.$fireModule
      //   .firestore()
      //   .collection('invitations')
      //   .doc(docId)
      //   .get()
      //   .then((res) => {
      //     return res.data()
      //   })
    },
    addNewGuest() {
      this.editedItem.guests.push({
        name: '',
        confirm: false,
      })
    },

    // Remove um convidado (Guest) sem alterar o banco
    removeGuest(key) {
      this.editedItem.guests.splice(key, 1)

      if (!this.editedItem.guests.length) {
        this.addNewGuest()
      }
    },

    editItem(item) {
      this.getInvite(item.id).then((res) => {
        this.editedIndex = this.invites.indexOf(item)
        this.editedItem.inviteName = item.id
        this.editedItem.guests = res.guests

        this.dialog = true
      })
    },

    // Abre a caixa de dialogo de deleção
    deleteItem(item) {
      this.editedIndex = this.invites.indexOf(item)
      this.editedItem.inviteName = item.id
      this.dialogDelete = true
    },

    // Deleta um convite (Invite)
    async deleteItemConfirm() {
      // this.invites.splice(this.editedIndex, 1)

      // await this.$fireModule
      //   .firestore()
      //   .collection('invitations')
      //   .doc(this.inviteNameFormatted)
      //   .delete()

      // this.getInvites()
      // this.close()
    },
    // Reseta o estado de todos os components
    close() {
      this.dialog = false
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    async save() {
      // if (this.editedIndex > -1) {
      //   // Update
      //   const invite = await this.$fireModule
      //     .firestore()
      //     .collection('invitations')
      //     .doc(this.inviteNameFormatted)

      //   invite.update({ guests: this.editedItem.guests })
      // } else {
      //   // Create
      //   await this.$fireModule
      //     .firestore()
      //     .collection('invitations')
      //     .doc(this.inviteNameFormatted)
      //     .set({ guests: this.editedItem.guests })
      // }

      // this.getInvites()
      // this.close()
    },
  },
}
</script>

<style lang="scss" scoped>
</style>
