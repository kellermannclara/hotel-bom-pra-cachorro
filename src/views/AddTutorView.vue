<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const novoTutor = ref({
    nome:'',
    telefone:'',
    id:'',
})
const tutores = ref([])
const router = useRouter();
const API_URL = 'http://localhost:3000'


async function carregarDados() {
  const respostasTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostasTutores.json();
}

async function salvarTutor() {
    await fetch(`${API_URL}/tutores`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(novoTutor.value)
        //JSON.stringify para converter de js pra json
    })

    //redireciona para listagem de pets
    router.push('/tutores')
}



onMounted(carregarDados)

</script>

<template>
    <div class="col-md-6">
        <label for="" class="form-label">
            Nome do Pet: 
        </label>
        <input class="form-control" type="text" id="nome" v-model="novoPet.nome" required/>
    </div>
</template>