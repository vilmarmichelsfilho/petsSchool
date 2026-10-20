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
  cpf: '',
  telefone: '',
  email: '',
  dataNascimento: '',
  id: '',
  rua: '',
  numero: '',
  bairro: '',
  cidade: '',
  estado: '',
  cep: '',
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

async function buscarCep() {
  const answer = await fetch(`https://viacep.com.br/ws/${novoTutor.value.cep}/json/`);

  const data = await answer.json();

  if (data.erro) {
    console.log("CEP não encontrado!");
    return;
  }

  novoTutor.value.rua = data.logradouro;
  novoTutor.value.bairro = data.bairro;
  novoTutor.value.cidade = data.localidade;
  novoTutor.value.estado = data.estado;
  novoTutor.value.cep = data.cep;
}

function formatarCPF() {
  let cpf = novoTutor.value.cpf;
  cpf = cpf.replace(/\D/g, "");
  cpf = cpf.substring(0, 11);
  cpf = cpf.replace(/(\d{3})(\d)/, "$1.$2");
  cpf = cpf.replace(/(\d{3})(\d)/, "$1.$2");
  cpf = cpf.replace(/(\d{3})(\d{1,2})$/, "$1-$2");

  novoTutor.value.cpf = cpf;
}

function formatarCEP() {
  let cep = novoTutor.value.cep;

  cep = cep.replace(/\D/g, "");
  cep = cep.substring(0, 8);
  cep = cep.replace(/(\d{5})(\d)/, "$1-$2");

  novoTutor.value.cep = cep;
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
    <h4 class="border-bottom pb-2">Dados do Tutor</h4>
    <div class="col-md-3">
        <label class="form-label">ID</label>
        <input required type="id" class="form-control" placeholder="Gerado Automaticamente" disabled>
    </div>
    <div class="col-md-9">
        <label class="form-label">Nome Completo</label>
        <input required type="text" class="form-control is-invalid" v-model="novoTutor.nome">
    </div>
    <div class="col-md-6">
      <label class="form-label">CPF / CNPJ</label>
      <input required @input="formatarCPF" class="form-control" type="text" pattern="\d{3}\.\d{3}\.\d{3}-\d{2}" placeholder="Digite o CPF ou CNPJ" v-model="novoTutor.cpf">
    </div>
    <div class="col-md-6">
        <label class="form-label">Telefone / Celular / Whatsapp</label>
        <input required type="tel" class="form-control" v-model="novoTutor.telefone">
    </div>
    <div class="col-md-6">
      <label class="form-label">E-mail</label>
      <input required type="email" v-model="novoTutor.email" class="form-control" placeholder="nome@example.com">
    </div>
    <div class="col-md-6">
      <label class="form-label">Data de Nascimento</label>
      <input required type="date" class="form-control" v-model="novoTutor.dataNascimento">
    </div>

    <h4 class="border-bottom pb-2">Endereço</h4>
    <div class="col-md-9">
      <label class="form-label">Rua</label>
      <input required type="text" class="form-control" placeholder="Digite o nome da rua" v-model="novoTutor.rua">
    </div>
    <div class="col-md-3">
      <label class="form-label">Número</label>
      <input required type="text" class="form-control" placeholder="Número" v-model="novoTutor.numero">
    </div>
    <div class="col-md-6">
      <label class="form-label">Bairro</label>
      <input required type="text" class="form-control" placeholder="Bairro" v-model="novoTutor.bairro">
    </div>
    <div class="col-md-6">
      <label class="form-label">Cidade</label>
      <input required type="text" class="form-control" placeholder="Cidade" v-model="novoTutor.cidade" disabled>
    </div>
    <div class="col-md-6">
      <label class="form-label">Estado</label>
      <input required type="text" v-model="novoTutor.estado" class="form-control" disabled></input>
    </div>
    <div class="col-md-6">
      <label class="form-label">CEP</label>
      <input required @input="formatarCEP" type="text" class="form-control" v-model="novoTutor.cep" placeholder="00000-000" @change="buscarCep">
    </div>

    <div class="col-12 d-flex gap-2">
        <button type="submit" class="btn btn-success"><i class="bi-floppy me-2"></i>Salvar Tutor</button>
    </div>
  </form>
</template>
