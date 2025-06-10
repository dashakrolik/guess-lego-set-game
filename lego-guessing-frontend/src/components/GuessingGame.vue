<template>
  <div v-if="legoSet">
    <img :src="legoSet.image_url" v-if="legoSet.image_url" width="300" />
    <p><strong>Year:</strong> {{ legoSet.year }}</p>
    <p><strong>Pieces:</strong> {{ legoSet.pieces }}</p>
    <p><strong>Theme:</strong> {{ legoSet.theme }}</p>

    <input
      v-model="guess"
      placeholder="Enter your guess"
      @keyup.enter="submitGuess"
    />
    <button @click="submitGuess">Submit</button>

    <p v-if="result === true">✅ Correct!</p>
    <p v-else-if="result === false">❌ Nope. Try again!</p>
  </div>

  <div v-else>
    <p>Loading LEGO set...</p>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

type LegoSet = {
  name: string
  year: number
  pieces: number
  theme: string
  image_url: string | null
}

const legoSet = ref<LegoSet | null>(null)
const guess = ref<string>('')
const result = ref<boolean | null>(null)

async function fetchSet() {
  const res = await fetch('http://localhost:8000/random-set')
  legoSet.value = await res.json()
  guess.value = ''
  result.value = null
}

async function submitGuess() {
  if (!guess.value.trim()) return

  const res = await fetch('http://localhost:8000/guess', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      guess: guess.value,
      answer: legoSet.value?.name,
    }),
  })

  const data = await res.json()
  result.value = data.correct

  if (data.correct) {
    setTimeout(fetchSet, 2000)
  }
}

onMounted(fetchSet)
</script>

<style scoped>
input {
  margin-top: 10px;
  padding: 8px;
  font-size: 16px;
  width: 100%;
  margin-bottom: 10px;
}
button {
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
}
</style>
