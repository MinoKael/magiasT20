<script setup>
import ModalCard from "./ModalCard.vue";
import BotaoOlho from "./BotaoOlho.vue";
import DetalhesMagia from "./DetalhesMagia.vue";
import { computed, reactive } from "vue";

const { prop, tomo } = defineProps(["prop", "tomo"]);
const isTrueOlho = computed(() => {
  return checkOlho(prop, tomo);
});

const checkOlho = (magia, array) => {
  if (array.some((el) => el.nome === magia.nome)) {
    return true;
  } else {
    return false;
  }
};
const snackbar = reactive({
  snackbar: false,
  text: "Magia adicionada ao Tomo.",
  timeout: 2000,
});
const emit = defineEmits(["sendMagia"]);
</script>
<template>
  <v-hover v-slot="{ isHovering, props }">
    <v-card
      class="ma-2 d-flex flex-column"
      :class="{ 'on-hover': isHovering }"
      :elevation="isHovering ? 12 : 6"
      max-width="300"
      hover
      v-bind="props"
    >
      <v-card-item class="pb-0">
        <v-card-title class="text-redTormenta text-wrap">{{
          prop.nome
        }}</v-card-title>
        <v-card-subtitle class="pb-2"
          >{{ prop.tipo }} {{ prop.circulo }} ({{ prop.escola }})
          <span class="text-redTormenta font-weight-black"
            >{{ prop.custo }} PM</span
          >
        </v-card-subtitle>
      </v-card-item>
      <v-card-text class="pb-1">
        <DetalhesMagia :prop="prop" />
      </v-card-text>
      <v-spacer />
      <BotaoOlho
        @click.stop="emit('sendMagia', prop)"
        @click.once="snackbar.snackbar = true"
        :prop="isTrueOlho"
      ></BotaoOlho>
      <ModalCard :prop="prop" />
      <v-snackbar
        v-model="snackbar.snackbar"
        :timeout="snackbar.timeout"
        color="redTormenta"
      >
        <p class="text-center text-button">
          {{ snackbar.text }}
        </p>
      </v-snackbar>
    </v-card>
  </v-hover>
</template>
<style scoped>
:deep(.v-card-subtitle) {
  opacity: 1 !important;
}
</style>
