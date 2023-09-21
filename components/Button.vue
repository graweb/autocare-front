<template>
  <v-tooltip bottom>
    <template v-slot:activator="{ on, attrs }">
      <v-btn elevation="2" v-bind="attrs" v-on="on" :color="color" v-bind:class="classButton" @click="$emit('click')"
        ><v-icon> {{ icon }} </v-icon></v-btn
      >
    </template>
    <span>{{ tooltip }}</span>
  </v-tooltip>
</template>

<script>
export default {
  props: {
    icon: String,
    tooltip: String,
    color: String,
    classButton: String,
  },
};
</script>
