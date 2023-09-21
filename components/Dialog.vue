<template>
  <v-row justify="center">
    <v-dialog v-model="show" persistent :max-width="maxWidth">
      <slot />
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  props: {
    visible: Boolean,
    maxWidth: String
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
