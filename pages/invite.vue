<template>
  <v-container>
    <v-form>
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
                <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-bind="attrs"
                  v-on="on"
                >
                  Novo convite
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="text-h5">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col></v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          v-model="editedItem.name"
                          label="Nome no convite"
                        ></v-text-field>
                      </v-col>
                      <v-col>
                        <v-row>
                          <v-col>
                            <v-text-field
                              v-model="editedItem.calories"
                              label="Convidado"
                            />
                          </v-col>
                          <v-col>
                            <v-checkbox
                              v-for="(guest, key) in inviteGuests"
                              :key="key"
                              v-model="inviteConfirmGuests"
                              :value="guest"
                              class="pa-2"
                              :label="guest"
                            />
                          </v-col>
                        </v-row>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field
                          v-model="editedItem.calories"
                          label="Calories"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field
                          v-model="editedItem.fat"
                          label="Fat (g)"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field
                          v-model="editedItem.carbs"
                          label="Carbs (g)"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field
                          v-model="editedItem.protein"
                          label="Protein (g)"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    Cancel
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template #item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
        </template>
        <template #no-data>
          <v-btn color="primary" @click="initialize"> Reset </v-btn>
        </template>
      </v-data-table>
    </v-form>
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
      invites: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      dialog: false,
    }
  },
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
  methods: {
    getInvites() {
      this.$fireModule
        .firestore()
        .collection('invitations')
        .get()
        .then((res) => {
          this.invites = res.docs
        })
    },
    editItem(item) {
      this.editedIndex = this.invites.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
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
