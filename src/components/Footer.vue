<template>
  <v-footer app bottom absolute padless class="bg-redTormenta">
    <v-row align="baseline" justify="center" no-gutters>
      {{ new Date().getFullYear() }} —
      <v-btn
        color="white"
        variant="text"
        class="pl-1"
        rounded="xl"
        append-icon="mdi-github"
        href="https://github.com/MinoKael"
        target="_blank"
        >MinoKael</v-btn
      >
      <v-col class="text-center mt-4" cols="12">
        Tormenta 20 é um produto da
        <v-btn
          variant="text"
          density="compact"
          href="https://jamboeditora.com.br/"
          target="_blank"
        >
          <strong>Jambô Editora</strong>
        </v-btn>
        e seus respectivos criadores, todos os direitos reservados. Este é um
        projeto de fã e não possui fins lucrativos.
      </v-col>
    </v-row>
  </v-footer>
</template>
