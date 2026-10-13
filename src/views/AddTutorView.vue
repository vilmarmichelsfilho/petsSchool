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
const novoTutor = ref({
  nome: '',
  telefone: '',
  id: '',
});

// search for all tutors that are saved into the application
async function loadTutors() {
  const answer = await fetch(`${API_URL}/tutores`);

  // convert data from my API from JSON to JS
  tutors.value = await answer.json();
  console.table(tutors.value);
}

// save tutor into the system
async function salvarTutor() {
    await fetch(`${API_URL}/tutores`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(novoTutor.value),
    });
    // redirect to pets list page
    router.push('/tutores');
}

onMounted(loadTutors);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastrar Tutor</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores cadastrados no sistema.
      </p>
    </header>
  </div>

  <form @submit.prevent="salvarTutor" class="row g-3">
    <div class="col-md-3">
        <label class="form-label">ID</label>
        <input type="id" class="form-control" placeholder="Gerado Automaticamente" disabled>
    </div>
    <div class="col-md-9">
        <label class="form-label">Nome Completo</label>
        <input type="text" class="form-control" v-model="novoTutor.nome">
    </div>
    <div class="col-md-6">
        <label class="form-label">Telefone / Celular / Whatsapp</label>
        <input type="tel" class="form-control" v-model="novoTutor.telefone">
    </div>
    <div class="col-12 d-flex gap-2">
        <button type="submit" class="btn btn-success"><i class="bi-floppy me-2"></i>Salvar Tutor</button>
    </div>
  </form>
</template>
