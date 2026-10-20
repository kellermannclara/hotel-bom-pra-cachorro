<script setup>

import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const tutores = ref([]);
const API_URL = 'http://localhost:3000';

async function carregarDados() {
  const respostasTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostasTutores.json();
}

async function excluirPet(id){
  const confirmarExclusao = window.confirm('Deseja excluir?');

  if(confirmarExclusao){
    await fetch(`${API_URL}/tutores/${id}`, {
      method: 'DELETE',
    });

    await carregarDados();
  }
}

onMounted(carregarDados);


</script>

<template>
     <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem de Tutores
    </p>
  </header>

  <table class="table table-striped table-hover">
    <thead>
      <tr>
        <th>id</th>
        <th>Nome</th>
        <th>Telefone</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="tutor in tutores"
        :key="tutor.id"
      >
        <td>{{ tutor.id }}</td>
        <td>{{ tutor.nome}}</td>
        <td>{{ tutor.telefone }}</td>
        <td>
          <RouterLink :to="`/tutores/${tutor.id}`">Vizualizar</RouterLink>
          <button class="btn btn-danger" @click="excluirPet(tutor.id)">Excluir</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>