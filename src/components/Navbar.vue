<script setup>
import { useTheme } from "vuetify";

const emit = defineEmits(["toggleMagias", "toggleTomo"]);

const theme = useTheme();
const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? "light" : "dark";
};
</script>
<template>
  <v-app-bar color="redTormenta" elevation="4">
    <v-responsive class="d-flex text-center">
      <v-btn-toggle mandatory="force">
        <v-btn color="white" variant="text" @click="emit('toggleMagias')"
          >Magias</v-btn
        >
        <v-btn color="white" variant="text" @click="emit('toggleTomo')"
          >Tomo</v-btn
        >
      </v-btn-toggle>

      <v-btn @click="toggleTheme">
        <v-icon>{{
          theme.global.current.value.dark
            ? "mdi-weather-sunny"
            : "mdi-weather-night"
        }}</v-icon>
      </v-btn>
    </v-responsive>
  </v-app-bar>
</template>
