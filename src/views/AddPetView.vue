<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

// calling router
const router = useRouter();

// receive API to list all the tutors
const tutors = ref([]);

// API const
const API_URL = 'http://localhost:3000';

// create object to save new pet
// properties: name, specie and tutorId
const newPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

// search for all tutors that are saved into the application
async function loadTutors() {
  const answer = await fetch(`${API_URL}/tutores`);

  // convert data from my API from JSON to JS
  tutors.value = await answer.json();
  console.table(tutors.value);
}

// save pet into the system
async function savePet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newPet.value),
  });
  // redirect to pets list page
  router.push('/pets');
}

onMounted(loadTutors);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>
  </div>

  <form @submit.prevent="savePet" class="row">
    
    <div class="col-md-6">
        <label for="nome" class="form-label">Nome do Pet</label>
        <input type="text" class="form-control" id="nome" v-model="newPet.nome" required>
    </div>

    <div class="col-md-6">
        <label for="especie" class="form-label">Espécie</label>
        <select name="especie" id="especie" class="form-select" required v-model="newPet.especie">
            <option value="" disabled>Selecione a espécie</option>
            <option value="Cachorro">Cachorro</option>
            <option value="Gato">Gato</option>
            <option value="Coelho">Coelho</option>
            <option value="Hamster">Hamster</option>
            <option value="Tartaruga">Tartaruga</option>
        </select>
    </div>

    <div class="col-md-6">
        <label for="tutor" class="form-label">Tutor</label>
        <select name="tutor" id="tutor" class="form-select" v-model="newPet.tutorId">
            <option value="" disabled>Selecione o tutor</option>
            <option v-for="tutor in tutors" :key="tutor.id" :value="tutor.id">{{ tutor.nome }}</option>
        </select>
    </div>

    <div class="col-12 d-flex gap-2">
        <button class="btn btn-success" type="submit">Salvar Pet</button>
    </div>
  </form>
</template>

<style scoped></style>
