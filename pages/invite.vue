<template>
  <v-container>
    {{ editedItem.inviteName }}
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
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-form :valid="editedItem.formValid">
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          v-model="editedItem.inviteName"
                          :value="editedItem.inviteName"
                          label="Nome no convite"
                        />
                      </v-col>
                      <v-col
                        v-for="(guest, key) in editedItem.guests"
                        :key="key"
                        cols="12"
                      >
                        <v-row>
                          <v-col cols="5">
                            {{ editedItem.guests[key].name }}
                            {{ key }}

                            <v-text-field
                              v-model="editedItem.guests[key].name"
                              :value="editedItem.guests[key].name"
                              label="Convidado"
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
                          <v-col
                            v-if="editedItem.guests.length === key + 1"
                            cols="2"
                          >
                            <v-btn @click="addNewGuest">
                              <v-icon>mdi-plus</v-icon>
                            </v-btn>
                          </v-col>
                        </v-row>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-form>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancelar
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Salvar </v-btn>
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
  data: () => ({
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
    invites: [],
    editedIndex: -1,
    editedItem: {
      inviteName: '',
      guests: [],
      formValid: false,
    },
    defaultItem: {
      inviteName: '',
      guests: [],
      formValid: false,
    },
    dialog: false,
  }),
  computed: {
    verifyUser() {
      return this.$fire.auth.currentUser
    },
    formTitle() {
      return this.editedIndex === -1 ? 'Novo convite' : 'Editar convite'
    },
  },
  mounted() {
    console.log(this)

    this.getInvites()
  },
  updated() {
    console.log(this)
  },
  methods: {
    async getInvites() {
      await this.$fireModule
        .firestore()
        .collection('invitations')
        .get()
        .then((res) => {
          this.invites = res.docs
        })
    },
    async getInvite(docId) {
      return await this.$fireModule
        .firestore()
        .collection('invitations')
        .doc(docId)
        .get()
        .then((res) => {
          return res.data()
        })
    },
    async addNewGuest() {
      console.log(this.editedItem)

      await this.$set(
        this.editedItem.guests,
        this.editedItem.guests.length,
        JSON.parse(
          JSON.stringify({
            name: '',
            confirm: false,
          })
        )
      )
    },
    async editItem(item) {
      await this.getInvite(item.id).then((res) => {
        console.log({ item, res })

        this.editedIndex = this.invites.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.editedItem.inviteName = 'aaaaaa'
        this.editedItem.guests = res.guests
        this.dialog = true
      })
    },
    deleteItem(item) {
      this.editedIndex = this.invites.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },
    deleteItemConfirm() {
      this.invites.splice(this.editedIndex, 1)
      this.closeDelete()
    },
    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>

<style lang="scss" scoped>
</style>
