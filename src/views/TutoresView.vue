<script setup>
import { RouterLink } from 'vue-router';
import { ref } from 'vue';
import { onMounted } from 'vue';

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

function getPets(tutorId) {
    const petsDesteTutor = ref([]);
        for (const pet of pets.value) {
                if (pet.tutorId === tutorId) {
                petsDesteTutor.value.push(pet.nome);
            };
        }
    return petsDesteTutor.value;
}

async function excluirTutor(id) {
  const confirmarExclusão = window.confirm('Deseja realmente excluir o pet?');

  if (confirmarExclusão) {
    await fetch(`${API_URL}/tutores/${id}`, {
      method: 'DELETE',
    });
    await loadData();
  }
}

onMounted(loadData);
</script>

<template>
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem dos Tutores</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos Tutores cadastrados no sistema.
    </p>
  </header>

  <RouterLink class="btn btn-primary" :to="'/tutores/novo'">Adicionar Tutor</RouterLink>

  <table class="table table-striped table-hover mt-3">
    <thead>
        <th>ID</th>
        <th>Nome</th>
        <th>Telefone</th>
        <th>Pets (Quantidade)</th>
        <th>Nome dos Pets</th>
        <th>Ações</th>
    </thead>
    <tbody>
        <tr v-for="tutor in tutors" :key="tutor.id">
            <td>{{ tutor.id }}</td>
            <td>{{ tutor.nome }}</td>
            <td>{{ tutor.telefone }}</td>
            <td>{{ getPets(tutor.id).length }}</td>
            <td>{{ getPets(tutor.id).join(' / ') }}</td>
            <td>
                <RouterLink class="btn btn-sm btn-outline-info me-2" :to="`/tutores/${tutor.id}`"><i class="bi-eye-fill me-1"></i>Visualizar</RouterLink>
                <button v-on:click.prevent="excluirTutor(tutor.id)" class="btn btn-sm btn-outline-danger"><i class="bi-trash me-1"></i>Excluir</button>
            </td>
        </tr>
    </tbody>
  </table>
</template>
