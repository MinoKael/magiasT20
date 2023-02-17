<script setup>
import DetalhesMagia from "./DetalhesMagia.vue";
import { ref } from "vue";
const dialog = ref(false);
const { prop } = defineProps(["prop"]);
</script>
<template>
  <v-dialog
    v-model="dialog"
    max-width="800px"
    scrollable
    transition="dialog-bottom-transition"
    activator="parent"
  >
    <v-card>
      <v-card-title
        class="text-h4 font-weight-bold text-redTormenta text-wrap px-6 pt-4"
        >{{ prop.nome }}</v-card-title
      >
      <v-card-subtitle class="font-weight-bold px-6">
        {{ prop.tipo }} {{ prop.circulo }} ({{ prop.escola }})
        <span class="text-redTormenta text-h6 font-weight-bold"
          >{{ prop.custo }} PM</span
        >
      </v-card-subtitle>
      <v-card-text>
        <DetalhesMagia :prop="prop" />

        <v-divider class="my-3"></v-divider>

        <p class="mt-2 text-pre-wrap">{{ prop.texto }}</p>

        <v-divider class="my-3"></v-divider>

        <p v-for="aprimoramento in prop.aprimoramentos">
          <span class="text-redTormenta font-weight-bold">{{
            aprimoramento.match(/.+?:/).toString()
          }}</span>
          {{ aprimoramento.match(/(?<=:).+/).toString() }}
        </p>
      </v-card-text>
      <v-card-actions>
        <v-btn color="redTormenta" block @click="dialog = false">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<style scoped>
:deep(.v-card-subtitle) {
  opacity: 1 !important;
}
</style>
