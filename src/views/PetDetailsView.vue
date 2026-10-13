<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import { RouterLink } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';
const pet = ref(null);
const tutor = ref(null);

async function carregarPet() {
    const idPet = route.params.id; // Get the pet ID from the route parameters

    // calling the API to get the pet details
    const respostaPet = await fetch(`${API_URL}/pets/${idPet}`);
    console.log('respostaPet', respostaPet);
    pet.value = await respostaPet.json();
    console.log('pet', pet.value);

    // calling the API to get the tutor details
    const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);
    console.log('respostaTutor', respostaTutor);
    tutor.value = await respostaTutor.json();
    console.log('tutor', tutor.value);
}

onMounted(carregarPet);
</script>

<template>
    <section v-if="pet == null">
        <p>Carregando...</p>
    </section>

    <section v-else>
        
        <RouterLink class="btn btn-ms btn-secondary mb-3" to="/pets"><i class="bi bi-arrow-left"></i> Retornar</RouterLink>

        <h1>Nome do Pet: {{ pet.nome }}</h1>
        <h5>Espécie: {{ pet.especie }}</h5>
        <h6>Tutor: {{ tutor.nome }}</h6>
    </section>
</template>

<style scoped>
</style>