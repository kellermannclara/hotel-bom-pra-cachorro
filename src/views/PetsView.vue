<script setup>
import { onMounted, ref } from 'vue';
// import { RouterLink } from 'vue-router';

const pets = ref([]);
const tutores = ref([]);
const API_URL = 'http://localhost:3000';

//chamar minha api para listar todos os pets e tutores
async function carregarDados() {
    console.log('PETS');
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  const respostasTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostasTutores.json();

  console.table(pets.value);
  console.table(tutores.value);
}

function nomeDoTutor(tutorId) {
  for (const tutor of tutores.value) {
    if (tutor.id === tutorId) {
      return tutor.nome;
    }
  }

  return 'Oops, Tutor não encontrado';
}

async function excluirPet(id) {
  const confirmarExclusao = window.confirm(`deseja realmente excluir o pet`)

  if (confirmarExclusao) {
    //LOGICA PARA EXCLUIR O PETT DO locALHOST3000
    await fetch(`${API_URL}/pets/${id}`, {
      method: 'DELETE',
    })

    await carregarDados()

  }
}

onMounted(carregarDados);

</script>

<template>
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos Pets cadastrados no sistema.
    </p>
  </header>

  <table class="table table-striped table-hover">
    <thead>
      <tr>
        <th>id</th>
        <th>Nome</th>
        <th>Espécie</th>
        <th>Tutor</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="pet in pets"
        :key="pet.id"
      >
        <td>{{ pet.id }}</td>
        <td>{{ pet.nome }}</td>
        <td>{{ pet.especie }}</td>
        <td>{{ nomeDoTutor(pet.tutorId) }}</td>
        <td>
          <RouterLink :to="`/pets/${pet.id}`">Vizualizar</RouterLink>
        </td>
        <td>
          <button class="btn btn-danger" @click="excluirPet(pet.id)">Excluir</button>
        </td>

      </tr>
    </tbody>
  </table>
</template>
