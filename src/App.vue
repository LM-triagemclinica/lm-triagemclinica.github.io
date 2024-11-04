<template>
  <div>
    <PaginaInicial v-if="currentPage === 'home'" @iniciar="irParaFormulario" />
    <!-- <Formulario v-else /> -->
    <FormularioTeste v-else />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import PaginaInicial from './components/PaginaInicial.vue';
import Formulario from './components/Formulario.vue';
import FormularioTeste from './components/FormularioTeste.vue';

const currentPage = ref('home');

const irParaFormulario = () => {
  currentPage.value = 'formulario';
};
</script>

<style scoped>

</style>