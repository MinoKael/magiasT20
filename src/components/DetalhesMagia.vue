<script setup>
const { prop } = defineProps(["prop"]);
</script>

<template>
  <span v-if="prop.execucao"
    ><strong>Execução: </strong> {{ prop.execucao }};&nbsp</span
  >
  <span v-if="prop.alcance"
    ><strong>Alcance: </strong> {{ prop.alcance }};&nbsp</span
  >
  <span v-if="prop.alvo"><strong>Alvo: </strong> {{ prop.alvo }};&nbsp</span>
  <span v-if="prop.area"><strong>Área: </strong> {{ prop.area }};&nbsp</span>
  <span v-if="prop.efeito"
    ><strong>Efeito: </strong> {{ prop.efeito }};&nbsp</span
  >
  <span v-if="prop.duracao"
    ><strong>Duração: </strong> {{ prop.duracao }};&nbsp</span
  >
  <span v-if="prop.resistencia"
    ><strong>Resistência: </strong> {{ prop.resistencia }};&nbsp</span
  >
</template>
