<template>
  <v-snackbar
    :color="snackbarColor"
    timeout="3000"
    v-model="showSnackbar"
    @input="closeSnackbar"
    top
    transition="scale-transition"
  >
    {{ snackbarText }}
    <v-btn color="white" text @click="closeSnackbar">Fechar</v-btn>
  </v-snackbar>
</template>

<script>
export default {
  props: {
    snackbarText: String,
    snackbarColor: String,
  },
  data() {
    return {
      showSnackbar: true,
    };
  },
  methods: {
    closeSnackbar() {
      this.showSnackbar = false;
      this.$emit("closeSnackbar");
    },
  },
};
</script>
