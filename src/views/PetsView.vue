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

// show tutor name
function tutorName(tutorId) {
for (const tutor of tutors.value) {
  if (tutor.id === tutorId) {
    return tutor.nome;
  }
}
return 'Oopps, Tutor not found!';
}

// function remove pet
async function excluirPet(id) {
  const confirmarExclusão = window.confirm('Deseja realmente excluir o pet?');

  if (confirmarExclusão) {
    await fetch(`${API_URL}/pets/${id}`, {
      method: 'DELETE',
    });
    await loadData();
  }
}

onMounted(loadData);
</script>

<template>
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos Pets cadastrados no sistema
    </p>
  </header>

  <RouterLink class="btn btn-primary" to="/pets/novo">Adicionar Pet</RouterLink>

  <table class="table table-striped table-hover mt-3">
    <thead>
      <th>Id</th>
      <th>Nome</th>
      <th>Espécie</th>
      <th>Tutor</th>  
      <th>Ações</th>
    </thead>
    <tbody>
      <tr v-for="pet in pets" :key="pet.id">
        <td>{{ pet.id }}</td>
        <td>{{ pet.nome }}</td>
        <td>{{ pet.especie }}</td>
        <td>{{ tutorName(pet.tutorId) }}</td>
        <td>
          <RouterLink :to="`/pets/${pet.id}`" class="btn btn-sm btn-outline-info me-2"><i class="bi-eye-fill me-1"></i>Visualizar</RouterLink>
          <button class="btn btn-sm btn-outline-danger" @click="excluirPet(pet.id)"><i class="bi-trash me-1"></i>Excluir</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped></style>
