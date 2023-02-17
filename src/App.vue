<script setup>
import Navbar from "./components/Navbar.vue";
import Magias from "./components/Magias.vue";
import Tomo from "./components/Tomo.vue";
import Footer from "./components/Footer.vue";
import { ref } from "vue";

const isMagias = ref(true);
const isTomo = ref(false);
const tomoMagias = ref([]);

const makeTomo = (magia) => {
  if (tomoMagias.value.some((tomo) => tomo.nome === magia.nome)) {
    const index = tomoMagias.value.indexOf(magia);
    tomoMagias.value.splice(index, 1);
  } else {
    tomoMagias.value.push(magia);
    tomoMagias.value = tomoMagias.value.sort(function (a, b) {
      return a.nome - b.nome;
    });
  }
};
</script>
<template>
  <v-app class="bg-pagina">
    <v-main>
      <Navbar
        @toggleMagias="
          isMagias = true;
          isTomo = false;
        "
        @toggleTomo="
          isMagias = false;
          isTomo = true;
        "
        :checkToggle="[isMagias, isTomo]"
      />
      <KeepAlive>
        <Magias v-if="isMagias" :tomo="tomoMagias" @emitTomo="makeTomo" />
      </KeepAlive>

      <Tomo
        v-if="isTomo"
        :tomo="tomoMagias"
        :prop="tomoMagias"
        @removeTomo="makeTomo"
      />

      <Footer />
    </v-main>
  </v-app>
</template>
