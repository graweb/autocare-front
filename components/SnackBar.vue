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

    <template v-slot:action="{ attrs }">
      <v-btn
        color="white"
        text
        v-bind="attrs"
        @click="closeSnackbar"
      >
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </template>
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
