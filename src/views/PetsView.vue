<script setup>
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

</template>