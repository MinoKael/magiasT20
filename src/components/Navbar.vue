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
    <v-row>
      <v-col cols="12" class="p-0 text-center h-100">
        <v-btn-toggle mandatory="force" class="text-center">
          <v-btn color="white" variant="text" @click="emit('toggleMagias')"
            >Magias</v-btn
          >
          <v-btn color="white" variant="text" @click="emit('toggleTomo')"
            >Tomo</v-btn
          >
        </v-btn-toggle>

        <v-btn icon @click="toggleTheme" aria-label="Mudar tema">
          <v-icon>{{
            theme.global.current.value.dark
              ? "mdi-weather-sunny"
              : "mdi-weather-night"
          }}</v-icon>
        </v-btn>
      </v-col>
    </v-row>
  </v-app-bar>
</template>
