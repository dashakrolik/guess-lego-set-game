<template>
  <main class="background">
    <GuessingGame />
  </main>
</template>

<script setup lang="ts">
import GuessingGame from './components/GuessingGame.vue'
</script>

<style scoped>
.background {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;

  background:
    linear-gradient(135deg, #201d48 0%, #201d48 30%, transparent 10%, transparent 100%),
    linear-gradient(315deg, #ffd502 0%, #ffd502 30%, transparent 30%, transparent 100%),
    white;

  background-repeat: no-repeat;
  background-attachment: fixed;
  font-family: 'Rubik', sans-serif;
}
</style>
