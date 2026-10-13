<script setup>
import { RouterLink } from 'vue-router';
import { onMounted, ref } from 'vue';

// receive API to list all the pets
const pets = ref([]);
const tutors = ref([]);

// API const
const API_URL = 'http://localhost:3000';
// function to receive the data
async function loadData() {
  const dataPets = await fetch(`${API_URL}/pets`);
  pets.value = await dataPets.json();

  const dataTutors = await fetch(`${API_URL}/tutores`);
  tutors.value = await dataTutors.json();

  console.log('PETS - ', pets.value);
  console.log('TUTORS - ', tutors.value);
}
onMounted(loadData);

// show tutor name
function tutorName(tutorId) {
for (const tutor of tutors.value) {
  if (tutor.id === tutorId) {
    return tutor.nome;
  }
}
return 'Oopps, Tutor not found!';
}
</script>

<template>
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos Pets cadastrados no sistema
    </p>
  </header>

  <RouterLink class="btn btn-primary" to="/pets/novo">Adicionar Pet</RouterLink>

  <table class="table table-striped table-hover">
    <thead>
      <th>Id</th>
      <th>Nome</th>
      <th>Espécie</th>
      <th>Tutor</th>  
    </thead>
    <tbody>
      <tr v-for="pet in pets" :key="pet.id">
        <td>{{ pet.id }}</td>
        <td>{{ pet.nome }}</td>
        <td>{{ pet.especie }}</td>
        <td>{{ tutorName(pet.tutorId) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped></style>
