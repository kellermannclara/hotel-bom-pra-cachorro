<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

//chamando meu rounter
const router = useRouter()
//chamando minha api
const API_URL = 'http://localhost:3000'
//tutores que vao ser cadrastados
const tutores = ref([])
//pbjeto pra salvar novo pet //// nome, especie e tutorId <= tem que ser os niomes iguais o que ta no banco de dados
const novoPet = ref({
    nome:'',
    especie:'',
    tutorId:'',
})
//buscar todos os tutores salvos na aplicação
async function carregarTutores() {
    const resposta = await fetch(`${API_URL}/tutores`)
    
    //convertendo dados da api de JSON para js
    tutores.value = await resposta.json()
    console.table(tutores.value)
}

//salvar um novo pet no sistema
async function salvarPet() {
    await fetch(`${API_URL}/pets`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(novoPet.value)
        //JSON.stringify para converter de js pra json
    })

    //redireciona para listagem de pets
    router.push('/pets')
}

onMounted(carregarTutores)
</script>


<template>
    <div></div>
    <header class="mb-4">
        <h1 class="text-2xl font-bold">
            Listagens De Pets
        </h1>
        <p class="text-body-secondary mb-0">
            Listagens dos Pets Cadastrados no Sistema
        </p>
    </header>

    <RouterLink class="btn btn-primary" :to="{name: 'novo-pet'}">
        Adicionar Pet
    </RouterLink>

    <form class="row" 
    @submit.prevent="salvarPet">

    <!-- nome do pet -->
    <div class="col-md-6">
        <label for="" class="form-label">
            Nome do Pet: 
        </label>
        <input class="form-control" type="text" id="nome" v-model="novoPet.nome" required/>
    </div>

    <!-- especie do troço -->
    <div class="col-md-6">
        <label class="form-label" for="especie">
            Espécie
        </label>
        <select name="especie" id="especie" class="form-select" required v-model="novoPet.especie">
            <option value="" disabled >Selecione a Espécie</option>
            <option value="Cachorro">Cachorro</option>
            <option value="Gato">Gato</option>
            <option value="Tartaruga">Tartaruga</option>
            <option value="Coelho">Coelho</option>
        </select>
    </div>

    <div class="col-md-6">
        <label for="tutor" class="form-label">Tutor</label>
        <select name="tutor" id="tutor" class="form-select" v-model="novoPet.tutorId">
            <option value="" disabled >Selecione o Tutor</option>
            <option v-for="tutor in tutores" :key="tutor.id" :value="tutor.id">
                {{ tutor.nome }}
            </option>
        </select>
    </div>

    <div class="col-12 d-flex gap-2">
        <button class="button btn-success" type="submit">
            Salvar Pet
        </button>
    </div>

    </form>

</template>