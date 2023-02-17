<script setup>
const { mdAndUp, filtroOpcoes, filtroPesquisa } = defineProps([
  "mdAndUp",
  "filtroOpcoes",
  "filtroPesquisa",
]);

const toNormal = (string) => {
  return string
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")
    .toLowerCase();
};
</script>
<template>
  <v-row no-gutters>
    <v-col
      v-for="(value, key) in filtroOpcoes"
      :key="key"
      cols="12"
      :style="mdAndUp ? 'flex: 0 0 20%; max-width:100%' : ''"
    >
      <v-responsive class="ma-1">
        <v-select
          :key="key"
          :label="key"
          :items="value"
          v-model="filtroPesquisa[toNormal(key)]"
          variant="solo"
          density="comfortable"
          bg-color="redTormenta"
          hide-details
        />
      </v-responsive>
    </v-col>
  </v-row>
</template>
