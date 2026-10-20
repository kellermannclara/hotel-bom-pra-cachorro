<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

//chamando meu rounter

const route = useRoute();
const API_URL = 'http://localhost:3000';
const tutor = ref(null);
const pets = ref([]);
const petTutor = ref([]);

async function carregar() {
  const idTutor = route.params.id;

  const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
  console.log('respostaTutor', respostaTutor);
  tutor.value = await respostaTutor.json();
  console.log('tutor', tutor.value);

  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  for (const pet of pets.value) {
        if (pet.tutorId === tutor.value.id) {
            petTutor.value.push(pet); 
        }
    }
}

onMounted(carregar);
</script>

<template>
  <section v-if="tutor == null">
    <p>caregandoooooo</p>
  </section>

  <section v-else>
    <h1>Nome do tutor: {{ tutor.nome }}</h1>
    <p>Telefone: {{ tutor.telefone }}</p>

    <div>
      <p class="text-body-secondary mb-0" >Pets deste Tutor</p>

      <div>
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>id</th>
              <th>Nome</th>
              <th>Espécie</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="pet in petTutor"
              :key="pet.id"
            >
              <td>{{ pet.id }}</td>
              <td>{{ pet.nome }}</td>
              <td>{{ pet.especie }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>
</template>
