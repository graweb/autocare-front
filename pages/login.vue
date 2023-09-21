<template>
  <v-flex sm12 md6 offset-md3>
    <v-card elevation="4">
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-card-title>
          <v-layout align-center justify-center>
            <h3 class="headline">AutoCare - Logo</h3>
          </v-layout>
        </v-card-title>
        <v-divider></v-divider>
        <v-card-text>
          <v-text-field
            v-model="email"
            label="E-mail"
            required
            :rules="emailRules"
          />
          <v-text-field
            v-model="password"
            :counter="10"
            label="Senha"
            :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
            :type="showPassword ? 'text' : 'password'"
            @click:append="showPassword = !showPassword"
            required
            :rules="passwordRules"
          />
          <v-checkbox v-model="remember" label="Lembrar dados" required />
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" text> Esqueceu a senha? </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" :disabled="!valid" @click.prevent="submit">
            <v-icon left>mdi-lock</v-icon>
            Entrar
          </v-btn>
        </v-card-actions>
      </v-form>
    </v-card>

    <SnackBar
      :snackbarText="snackbarText"
      :snackbarColor="snackbarColor"
      v-if="showSnackbar"
      @closeSnackbar="showSnackbar = false"
    />
  </v-flex>
</template>

<script>
export default {
  layout: "guest",

  data: () => ({
    email: "",
    password: "",
    remember: false,
    showPassword: false,
    valid: true,
    emailRules: [
      (v) => !!v || "E-mail é obrigatório.",
      (v) => /.+@.+\..+/.test(v) || "Digite um e-mail válido.",
    ],
    passwordRules: [
      (v) => !!v || "Senha é obrigatório.",
      (v) =>
        (v && v.length <= 10) || "A senha deve ter no máximo 10 caracteres.",
    ],
    showSnackbar: false,
    snackbarText: "",
    snackbarColor: "",
  }),

  methods: {
    async submit() {
      if (this.$refs.form.validate()) {
        await this.$auth
          .loginWith("laravelSanctum", {
            data: {
              email: this.email,
              password: this.password,
              remember: this.remember,
            },
          })
          .then(() => {
            this.router.push("/");
          })
          .catch((err) => {
            if(err.response) {
              this.snackbarText = err.response.data.message;
              this.snackbarColor = "error";
              this.showSnackbar = true;
            }
          });
      }
    },
  },
};
</script>
