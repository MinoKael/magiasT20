<script setup>
import { ref, watch, reactive } from "vue";
import m from "../data/Magias.json";
import Card from "./Card.vue";
import { useDisplay } from "vuetify";
import Filtros from "./Filtros.vue";

const magias = ref(m);
const filtroPesquisa = reactive({
  nome: "",
  tipo: "Qualquer",
  circulo: "Qualquer",
  escola: "Qualquer",
  execucao: "Qualquer",
  alcance: "Qualquer",
  alvo: "Qualquer",
  area: "Qualquer",
  efeito: "Qualquer",
  duracao: "Qualquer",
  resistencia: "Qualquer",
});

const filtroOpcoes = reactive({
  Tipo: ["Qualquer", "Arcana", "Divina", "Universal"],
  Círculo: ["Qualquer", 1, 2, 3, 4, 5],
  Escola: [
    "Qualquer",
    "Abjuração",
    "Adivinhação",
    "Convocação",
    "Encantamento",
    "Evocação",
    "Ilusão",
    "Necromancia",
    "Transmutação",
  ],
  Execução: [
    "Qualquer",
    "Livre",
    "Reação",
    "Padrão",
    "Movimento",
    "Completa",
    "Outros",
  ],
  Alcance: [
    "Qualquer",
    "Pessoal",
    "Toque",
    "Curto",
    "Médio",
    "Longo",
    "Ilimitado",
    "Outros",
  ],
  Alvo: ["Qualquer", "Criatura", "Objeto", "Você", "Outros"],
  Área: [
    "Qualquer",
    "Cilindro",
    "Cone",
    "Esfera",
    "Linha",
    "Quadrado",
    "Outros",
  ],
  Efeito: ["Qualquer", "Efeitos"],
  Duração: [
    "Qualquer",
    "Instantânea",
    "Cena",
    "Sustentada",
    "Descarregada",
    "Outros",
  ],
  Resistência: ["Qualquer", "Fortitude", "Reflexos", "Vontade", "Outros"],
});

watch(filtroPesquisa, () => {
  magias.value = m.filter(function (magia) {
    // LÓGICA FILTRO EXECUÇÃO
    const execucao = ref(null);
    filtroPesquisa.execucao == "Outros"
      ? (execucao.value = ![
          /livre/,
          /reação/,
          /padrão/,
          /movimento/,
          /completa/,
        ].some((rx) => rx.test(magia.execucao)))
      : (execucao.value = magia.execucao
          .toLowerCase()
          .includes(replaceQualquer(filtroPesquisa.execucao).toLowerCase()));

    // LÓGICA FILTRO ALCANCE
    const alcance = ref(null);
    filtroPesquisa.alcance == "Outros"
      ? (alcance.value = ![
          /^$/,
          /pessoal/,
          /toque/,
          /curto/,
          /médio/,
          /longo/,
          /ilimitado/,
        ].some((rx) => rx.test(magia.alcance)))
      : (alcance.value = magia.alcance
          .toLowerCase()
          .includes(replaceQualquer(filtroPesquisa.alcance).toLowerCase()));

    // LÓGICA FILTRO ALVO
    const alvo = ref(null);
    filtroPesquisa.alvo == "Outros"
      ? (alvo.value = ![/^$/, /criatura/, /objeto/, /você/].some((rx) =>
          rx.test(magia.alvo)
        ))
      : (alvo.value = magia.alvo.includes(
          replaceQualquer(filtroPesquisa.alvo).toLowerCase()
        ));

    // LÓGICA FILTRO ÁREA
    const area = ref(null);
    filtroPesquisa.area == "Outros"
      ? (area.value = ![
          /^$/,
          /cilindro/,
          /cone/,
          /esfera/,
          /linha/,
          /quadrado/,
        ].some((rx) => rx.test(magia.area)))
      : (area.value = magia.area.includes(
          replaceQualquer(filtroPesquisa.area).toLowerCase()
        ));

    // LÓGICA FILTRO EFEITO
    const efeito = ref(null);
    filtroPesquisa.efeito == "Qualquer"
      ? (efeito.value = /.*/.test(magia.efeito))
      : (efeito.value = /\S/.test(magia.efeito));

    // LÓGICA FILTRO DURAÇÃO
    const duracao = ref(null);
    filtroPesquisa.duracao == "Outros"
      ? (duracao.value = ![
          /^$/,
          /instantânea/,
          /cena/,
          /sustentada/,
          /descarregada/,
        ].some((rx) => rx.test(magia.duracao)))
      : (duracao.value = magia.duracao.includes(
          replaceQualquer(filtroPesquisa.duracao).toLowerCase()
        ));
    // LÓGICA FILTRO RESISTÊNCIA
    const resistencia = ref(null);
    filtroPesquisa.resistencia == "Outros"
      ? (resistencia.value = ![/^$/, /fortitude/, /reflexos/, /vontade/].some(
          (rx) => rx.test(magia.resistencia.toLowerCase())
        ))
      : (resistencia.value = magia.resistencia
          .toLowerCase()
          .includes(replaceQualquer(filtroPesquisa.resistencia).toLowerCase()));

    return (
      toNormal(magia.nome).includes(filtroPesquisa.nome.toLowerCase()) &&
      magia.tipo.includes(replaceQualquer(filtroPesquisa.tipo)) &&
      magia.circulo
        .toString()
        .includes(replaceQualquer(filtroPesquisa.circulo)) &&
      magia.escola.includes(replaceQualquer(filtroPesquisa.escola)) &&
      execucao.value &&
      alcance.value &&
      alvo.value &&
      area.value &&
      efeito.value &&
      duracao.value &&
      resistencia.value
    );
  });
});

const toNormal = (string) => {
  return string
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")
    .toLowerCase();
};

const limparFiltros = () => {
  for (let [key] of Object.entries(filtroPesquisa)) {
    key === "nome"
      ? (filtroPesquisa[key] = "")
      : (filtroPesquisa[key] = "Qualquer");
  }
};
const replaceQualquer = (str) => {
  return str === "Qualquer" ? "" : str;
};
const emit = defineEmits(["emitTomo"]);

const tomo = defineProps(["tomo"]);

const { mdAndUp } = useDisplay();
</script>
<template>
  <!-- MAGIAS CONTAINER -->
  <v-container class="fill-height justify-center">
    <v-responsive
      class="text-center fill-height"
      :max-width="mdAndUp ? '1000' : ''"
    >
      <h1 class="text-h3 text-redTormenta font-weight-bold mb-2">
        Magias Tormenta 20
      </h1>
      <!-- BARRA PESQUISA -->
      <v-responsive class="mx-0 pa-1">
        <v-text-field
          bg-color="redTormenta"
          hide-details
          label="Magia"
          placeholder="Magia"
          v-model.trim="filtroPesquisa.nome"
          filled
          rounded
          dense
          single-line
          class="mx-4 custom-placeholer"
        >
        </v-text-field>
      </v-responsive>

      <!-- FILTROS -->
      <v-container class="d-flex flex-wrap pt-1 pb-1">
        <Filtros
          v-if="mdAndUp"
          :mdAndUp="mdAndUp"
          :filtroOpcoes="filtroOpcoes"
          :filtroPesquisa="filtroPesquisa"
        />

        <v-expansion-panels v-else class="mx-1 pb-1" color="redTormenta">
          <v-expansion-panel title="Filtros">
            <v-expansion-panel-text>
              <Filtros
                :mdAndUp="mdAndUp"
                :filtroOpcoes="filtroOpcoes"
                :filtroPesquisa="filtroPesquisa"
              />
            </v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-container>
      <v-btn color="redTormenta" @click="limparFiltros">Limpar Filtros</v-btn>
      <h4 class="mt-2 pa-0">
        {{ magias.length }} de {{ m.length }} magias encontradas
      </h4>
      <!-- CARDS CONTAINER -->

      <v-container class="d-flex flex-wrap justify-center pt-1">
        <Card
          v-for="magia in magias"
          :key="magia.id"
          :prop="magia"
          :tomo="tomo.tomo"
          @sendMagia="emit('emitTomo', magia)"
          v-bind="$attrs"
        />
      </v-container>
    </v-responsive>
  </v-container>
</template>
<style scoped>
:deep(.v-label) {
  color: white !important;
  opacity: 1;
  font-weight: bolder;
}
:deep(.custom-placeholer) {
  font-weight: 100 !important;
}
:deep(.v-field--active) {
  font-weight: 400 !important;
}
</style>
