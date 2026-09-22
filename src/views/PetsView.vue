<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos Pets cadastrados no sistema
    </p>
  </header>

  <RouterLink class="btn btn-primary" to="/pets/novo">Adicionar Pet</RouterLink>
</template>

<style scoped></style>
