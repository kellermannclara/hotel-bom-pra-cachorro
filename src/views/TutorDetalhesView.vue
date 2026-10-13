<script setup>

import { onMounted, ref } from 'vue';
import useRoute from '@/router';

const tutores = ref(null);
const route = useRoute();
const API_URL = 'http://localhost:3000';

async function carregarDados() {
    const tutorId = route.params.id
  const respostasTutores = await fetch(`${API_URL}/tutores/${tutorId}`);
  tutores.value = await respostasTutores.json();
}

onMounted(carregarDados);

</script>

<template>
    <section v-if="tutores == null">
    <p>CARREGANDO......</p>
  </section>

  <section v-else>
    <h1>Nome do Tutor: {{ tutor.nome }}</h1>
    <h5>Telefone: {{ tutor.telefone }}</h5>
  </section>
</template>