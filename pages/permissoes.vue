<template>
  <div>
    <v-data-table
      :headers="headers"
      v-model="selectedItems"
      :items="items"
      show-select
      @click:row="rowClick"
      loading-text="Carregando..."
      :search="search"
      :loading="loading"
      item-value="id"
      item-key="id"
      return-object
    >
      <template v-slot:top>
        <v-toolbar flat>
          <Button
            icon="mdi-plus"
            tooltip="Novo"
            classButton="mr-0"
            @click="openModal"
          />
          <Button
            icon="mdi-pencil"
            tooltip="Editar"
            v-if="selectedItems.length == 1"
            classButton="ml-2"
            @click="editDialog"
          />
          <Button
            icon="mdi-trash-can-outline"
            tooltip="Excluir"
            color="red lighten-4"
            v-if="selectedItems.length >= 1"
            classButton="ml-2"
            @click="dialogRemove = true"
          />
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Buscar"
            single-line
            hide-details
            class="ml-4"
          />
        </v-toolbar>
      </template>
    </v-data-table>

    <SnackBar
      :snackbarText="snackbarText"
      :snackbarColor="snackbarColor"
      v-if="showSnackbar"
      @closeSnackbar="showSnackbar = false"
    />

    <Dialog :visible="dialog" @close="dialog = false" maxWidth="600px">
      <v-card>
        <v-form ref="form" lazy-validation>
          <v-card-title>
            <span class="text-h5">{{ modalTitle }}</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="12">
                  <v-text-field
                    v-model="data.name"
                    label="Permissão:"
                    :rules="nameRules"
                    required
                  />
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="red darken-1" text @click="dialog = false">
              Fechar
            </v-btn>
            <v-btn color="blue darken-1" dark @click="save"> Salvar </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </Dialog>

    <Dialog
      :visible="dialogRemove"
      @close="dialogRemove = false"
      maxWidth="400px"
    >
      <v-card>
        <v-card-title class="text-h5"> Atenção </v-card-title>
        <v-card-text>
          <h3>Deseja remover os itens abaixo?</h3>
        </v-card-text>
        <v-card-subtitle
          v-for="item in selectedItems"
          :key="item.id"
          class="pb-0"
        >
          {{ item.name }}
        </v-card-subtitle>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="dialogRemove = false">
            Não
          </v-btn>
          <v-btn color="primary" @click="saveDelete"> Sim </v-btn>
        </v-card-actions>
      </v-card>
    </Dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      headers: [{ text: "PERMISSÃO", value: "name" }],
      search: "",
      items: [],
      selectedItems: [],
      loading: false,
      showSnackbar: false,
      snackbarText: "",
      snackbarColor: "",
      dialog: false,
      dialogRemove: false,
      editedIndex: -1,
      modalTitle: "",
      data: {
        id: 0,
        name: "",
      },
      nameRules: [(v) => !!v || "Informe o nome da permissão"],
    };
  },

  async fetch() {
    this.loading = true;

    await this.$axios
      .get(process.env.apiUrl + "permissions")
      .then((res) => {
        this.selectedItems = [];
        this.data = {};
        this.loading = false;
        this.items = res.data;
      })
      .catch(function (err) {
        this.loading = false;
        this.snackbarText = err;
        this.snackbarColor = "error";
        this.showSnackbar = true;
      });
  },

  methods: {
    openModal() {
      this.selectedItems = [];
      this.data.id = 0;
      (this.modalTitle = "Nova Permissão"), (this.dialog = true);
    },

    editDialog() {
      (this.modalTitle = "Editar Permissão"),
        (this.editedIndex = this.items.indexOf(this.selectedItems));
      this.data = Object.assign({}, this.selectedItems[0]);
      this.dialog = true;
    },

    rowClick(item, row) {
      let selectState = row.isSelected ? false : true;
      row.select(selectState);
    },

    async save() {
      if (this.$refs.form.validate()) {
        if (this.data.id == 0) {
          await this.$axios
          .post(process.env.apiUrl + "permissions", {
            name: this.data.name,
          })
          .then(() => {
            this.dialog = false;
            this.snackbarText = "Permissão registrada com sucesso!";
            this.snackbarColor = "success";
            this.showSnackbar = true;
            this.$fetch();
          })
          .catch(function (err) {
            this.snackbarText = err;
            this.snackbarColor = "error";
            this.showSnackbar = true;
          });
        } else {
          await this.$axios
          .put(process.env.apiUrl + "permissions/" + this.data.id, {
            name: this.data.name,
          })
          .then(() => {
            this.dialog = false;
            this.snackbarText = "Permissão atualizada com sucesso!";
            this.snackbarColor = "success";
            this.showSnackbar = true;
            this.$fetch();
          })
          .catch(function (err) {
            this.snackbarText = err;
            this.snackbarColor = "error";
            this.showSnackbar = true;
          });
        }
      }
    },

    saveDelete() {
      for (var i = 0; i < this.selectedItems.length; i++) {
        this.$axios
          .delete(
            process.env.apiUrl + "permissions/" + this.selectedItems[i].id
          )
          .then(() => {
            this.dialogRemove = false;
            this.snackbarText = "Permissão removida com sucesso!";
            this.snackbarColor = "success";
            this.showSnackbar = true;
            this.$fetch();
          })
          .catch(function (err) {
            this.snackbarText = err;
            this.snackbarColor = "error";
            this.showSnackbar = true;
          });
      }
    },
  },
};
</script>
