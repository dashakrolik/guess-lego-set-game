<template>
  <main class="container">
    <h1>🧩 LEGO Set Guessing Game</h1>
    <GuessingGame />
  </main>
</template>

<script setup lang="ts">
import GuessingGame from './components/GuessingGame.vue'
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  font-family: sans-serif;
}
</style>
