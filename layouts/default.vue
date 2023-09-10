<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" fixed app dark>
      <v-list nav>
        <v-list-item @click="openModal">
          <v-list-item-title>
            <v-icon>mdi-plus</v-icon>
            Novo Veículo
          </v-list-item-title>
        </v-list-item>
      </v-list>

      <v-divider />

      <v-list nav>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <v-divider />
        <v-menu offset-y :nudge-width="-25" transition="slide-y-transition">
          <template v-slot:activator="{ on, attrs }">
            <v-list-item link v-bind="attrs" v-on="on">
              <v-list-item-avatar>
                <v-icon dark size="36"> mdi-account-circle </v-icon>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>{{ $auth.user.name }}</v-list-item-title>

                <v-list-item-subtitle>{{
                  $auth.user.email
                }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-action>
                <v-icon color="grey lighten-1">mdi-dots-horizontal</v-icon>
              </v-list-item-action>
            </v-list-item>
          </template>

          <v-list nav>
            <v-list-item to="/">
              <v-list-item-title>
                <v-icon>mdi-cog</v-icon>
                Configurações
              </v-list-item-title>
            </v-list-item>
            <v-list-item @click="logout">
              <v-list-item-title>
                <v-icon>mdi-logout</v-icon>
                Sair
              </v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </template>
    </v-navigation-drawer>

    <v-app-bar fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-btn icon @click="logout">
        <v-icon>mdi-share</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <Dialog :title="'Novo Veículo'" :visible="dialog" @close="dialog = false">
      <v-card>
        <v-form ref="form" lazy-validation>
          <v-card-title>
            <span class="text-h5">Novo Veículo</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="12">
                  <v-text-field
                    label="Placa:"
                    :counter="7"
                    maxlength="7"
                    required
                    v-model="plate"
                    :rules="vehiclePlateRules"
                    :append-outer-icon="'mdi-magnify'"
                    @click:append-outer="searchVehicle"
                    :loading="loading"
                    :disabled="loading"
                    hint="Digite a placa e clique em pesquisar..."
                  />
                </v-col>
              </v-row>
              <v-row v-if="showResult">
                <v-col cols="12" sm="6" md="4">
                  <v-text-field
                    v-model="form.vehicleBrand"
                    label="Marca:"
                    readonly
                  />
                </v-col>
                <v-col cols="12" sm="6" md="8">
                  <v-text-field
                    v-model="form.vehicleModel"
                    label="Modelo:"
                    readonly
                  />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field
                    v-model="form.vehicleColor"
                    label="Cor:"
                    readonly
                  />
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    v-model="form.vehicleYear"
                    label="Ano:"
                    readonly
                  />
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    v-model="form.vehicleYearModel"
                    label="Ano Modelo:"
                    readonly
                  />
                </v-col>
                <v-col cols="12" sm="6" md="2">
                  <v-text-field
                    v-model="form.vehicleVersion"
                    label="Versão:"
                    readonly
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
            <v-btn
              color="blue darken-1"
              dark
              @click.prevent="saveVehicle"
              v-if="showResult"
            >
              Salvar
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </Dialog>

    <SnackBar
      :snackbarText="snackbarText"
      :snackbarColor="snackbarColor"
      v-if="showSnackbar"
      @closeSnackbar="showSnackbar = false"
    />
  </v-app>
</template>

<script>
import SnackBar from "@/components/SnackBar.vue";

export default {
  name: "DefaultLayout",

  components: {
    SnackBar,
  },

  data() {
    return {
      drawer: true,
      dialog: false,
      plate: "",
      showSnackbar: false,
      snackbarText: "",
      snackbarColor: "",
      loading: false,
      showResult: false,
      form: {
        vehicleBrand: "",
        vehicleModel: "",
        vehicleVersion: "",
        vehicleYear: "",
        vehicleYearModel: "",
        vehicleColor: "",
        vehicleChassi: "",
        vehicleFuel: "",
        vehicleMotor: "",
        vehicleNationality: "",
        vehicleUf: "",
        vehicleCity: "",
        vehicleNumberOfPassengers: ""
      },
      vehiclePlateRules: [
        (v) => !!v || "Informe a placa",
        (v) => v.length >= 7 || "A placa precisar ter 7 caracateres",
      ],
      items: [
        {
          icon: "mdi-home",
          title: "Início",
          to: "/",
        },
        {
          icon: "mdi-car",
          title: "Veículos",
          to: "/veiculos",
        },
        {
          icon: "mdi-cogs",
          title: "Manutenção",
          to: "/manutencao",
        },
        {
          icon: "mdi-tow-truck",
          title: "Oficinas",
          to: "/oficinas",
        },
        {
          icon: "mdi-cash-multiple",
          title: "Despesas",
          to: "/despesas",
        },
      ],
    };
  },

  methods: {
    openModal() {
      this.plate = "";
      this.showResult = false;
      this.dialog = true;
    },

    async searchVehicle() {
      if (this.$refs.form.validate()) {
        this.loading = true;

        await this.$axios
          .post(process.env.apiUrl + "api_brasil_vehicles", {
            placa: this.plate,
          })
          .then((res) => {
            if(!res.data.response || res.data.response == null || res.data.response == null) {
              this.snackbarText = "Favor, informe uma palca válida para realizar a pesquisa!";
              this.snackbarColor = "error";
              this.showSnackbar = true;
              this.loading = false;
            } else if (
              res.data.response.message ==
              "Placa Invalida favor usar o formato AAA0X00 ou AAA9999 "
            ) {
              this.snackbarText = res.data.response.message;
              this.snackbarColor = "error";
              this.showSnackbar = true;
              this.loading = false;
            } else {
              this.form.vehicleBrand = res.data.response.marca;
              this.form.vehicleModel = res.data.response.modelo;
              this.form.vehicleVersion = res.data.response.VERSAO;
              this.form.vehicleYear = res.data.response.ano;
              this.form.vehicleYearModel = res.data.response.anoModelo;
              this.form.vehicleColor = res.data.response.cor;
              this.form.vehicleChassi = res.data.response.extra.chassi,
              this.form.vehicleFuel = res.data.response.extra.combustivel.combustivel,
              this.form.vehicleMotor = res.data.response.extra.motor,
              this.form.vehicleNationality = res.data.response.origem,
              this.form.vehicleUf = res.data.response.uf,
              this.form.vehicleCity = res.data.response.municipio,
              this.form.vehicleNumberOfPassengers = res.data.response.extra.quantidade_passageiro,
              this.showResult = true;
            }
            this.loading = false;
          })
          .catch(function (err) {
            this.loading = false;
            this.snackbarText = err;
            this.snackbarColor = "error";
            this.showSnackbar = true;
          });
      }
    },

    async saveVehicle() {
      await this.$axios
        .post(process.env.apiUrl + "vehicles", {
          'plate': this.plate,
          'brand': this.form.vehicleBrand,
          'model': this.form.vehicleModel,
          'color': this.form.vehicleColor,
          'year': this.form.vehicleYear,
          'year_model': this.form.vehicleYearModel,
          'version': this.form.vehicleVersion,
          'chassi': this.form.vehicleChassi,
          'fuel': this.form.vehicleFuel,
          'motor': this.form.vehicleMotor,
          'nationality': this.form.vehicleNationality,
          'uf': this.form.vehicleUf,
          'city': this.form.vehicleCity,
          'number_of_passengers': this.form.vehicleNumberOfPassengers,
        })
        .then(() => {
          this.dialog = false;
          this.snackbarText = "Veículo registrado com sucesso!";
          this.snackbarColor = "success";
          this.showSnackbar = true;
        })
        .catch(function (err) {
          this.snackbarText = err;
          this.snackbarColor = "error";
          this.showSnackbar = true;
        });
    },

    async logout() {
      await this.$auth
        .logout()
        .then(() => {
          this.$router.push("/login");
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>
