<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import { RouterLink } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';
const tutor = ref(null);
const pets = ref(null);

async function carregarPet() {
    const idTutor = route.params.id; // Get the pet ID from the route parameters

    // calling the API to get the tutor details
    const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
    console.log('respostaPet', respostaTutor);
    tutor.value = await respostaTutor.json();
    console.log('tutor', tutor.value);

    // calling the API to get the pet details
    const dataPets = await fetch(`${API_URL}/pets`);
    pets.value = await dataPets.json();
}

function getPets(tutorId) {
    const petsDesteTutor = ref([]);
        for (const pet of pets.value) {
                if (pet.tutorId === tutorId) {
                petsDesteTutor.value.push(pet.id);
            };
        }
    return petsDesteTutor.value;
}

onMounted(carregarPet);
</script>

<template>
    <section v-if="tutor == null">
        <p>Carregando...</p>
    </section>

    <section v-else>
        <RouterLink to="/tutores" class="btn btn-outline-primary mb-3"><i class="bi-arrow-left me-1 position-absolute top-0 end-0 m-3"></i>Voltar para Tutores</RouterLink>

    <h2>Detalhes do Tutor</h2>
    <p class="text-secondary">Visualize os dados do tutor e seus pets vinculados.</p>

    <div class="card mb-5">
        <div class="card-header">
            <h5>Dados Tutor</h5>
        </div>
        <div class="card-body d-flex justify-content-between">
            <div>
                <label style="font-weight: bolder;">ID</label>
                <p>{{ tutor.id }}</p>
            </div>
            <div>
                <label style="font-weight: bolder;">Nome Completo</label>
                <p>{{ tutor.nome }}</p>
            </div>
            <div>
                <label style="font-weight: bolder;">Telefone / Celular / Whatsapp</label>
                <p>{{ tutor.telefone }}</p>
            </div>
        </div>
    </div>

    <div class="card">
        <div class="card-header d-flex align-items-center gap-2">
            <h5>Pets Vinculados</h5>
            <span class="badge rounded-pill text-bg-primary">{{ getPets(tutor.id).length }} pets</span>
        </div>
        <div class="card-body">
            <table class="table table-striped table-hover mt-3">
                <thead>
                    <th>ID</th>
                    <th>Nome</th>
                    <th>Espécie</th>
                    <th>Ações</th>
                </thead>
                <tbody>
                    <tr v-for="pet in Array.from(getPets(tutor.id))" :key="pet.id">
                        <td>{{ pets[pets.findIndex(i => i.id === pet)].id }}</td>
                        <td>{{ pets[pets.findIndex(i => i.id === pet)].nome }}</td>
                        <td>{{ pets[pets.findIndex(i => i.id === pet)].especie }}</td>
                        <td><RouterLink :to="`/pets/${pets[pets.findIndex(i => i.id === pet)].id}`" class="btn btn-sm btn-outline-info">Vizualizar Pet</RouterLink></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    </section>
</template>