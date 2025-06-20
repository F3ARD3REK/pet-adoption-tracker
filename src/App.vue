<template>
  <div class="app">
    <h1>Animal Adoption Tracker</h1>
    <!-- main component here -->
    <PetList />
  </div>
</template>

<script setup>
// importing the pet list component
import PetList from './components/PetList.vue'
</script>

<style>
/* Basic styling — nothing fancy for now */
body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  color: #222;
  margin: 0;
  padding: 20px;
}

/* might want to tweak this later */
.app {
  max-width: 800px;
  margin: auto;
}
</style>
