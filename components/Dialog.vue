<template>
  <v-row justify="center">
    <v-dialog v-model="show" persistent max-width="600px">
      <slot />
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  props: {
    visible: Boolean,
  },

  computed: {
    show: {
      get() {
        return this.visible;
      },
      set(value) {
        if (!value) {
          this.$emit("close");
        }
      },
    },
  },
};
</script>
