<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const pets = ref([]);
const tutores = ref([])

//chamar minha api para listar todos os pets e tutores
async function carregarDados() {
    const respostaPets = await fetch(`${API_URL}/pets`)
    pets.value = await respostaPets.json()

    const respostasTutores = await fetch(`${API_URL}/tutores`)
    tutores.value = await respostasTutores.json()

    console.log('PETS -', pets.value)
    console.log('TUTORES -', tutores.value)
}
onMounted(carregarDados)

</script>


<template>
    <div></div>
      <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <table class="table table-striped table-hover">
        <thead>
            <th>id</th>
            <th>Nome</th>
            <th>Espécie</th>
            <th>Tutor</th>
        </thead>
        <tbody>
            <tr v-for="pet in pets" :key="pet.id">
                <td>{{ pet.id }}</td>
                <td>{{ pet.nome }}</td>
                <td>{{ pet.especie }}</td>
                <td>{{ pet.tutorId }}</td>
            </tr>
        </tbody>
    </table>

    <RouterLink class="btn btn-primary" :to="{name: 'novo-pet'}">
        Adicionar Pet
    </RouterLink>

</template>