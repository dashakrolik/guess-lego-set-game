<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface LegoSet {
  name: string
  year: number
  pieces: number
  theme: string
  image_url?: string
}

const set = ref<LegoSet | null>(null)
const guess = ref('')
const feedback = ref<'correct' | 'wrong' | null>(null)
const showAnswer = ref(false)
const isLoading = ref(false)

async function fetchSet() {
  isLoading.value = true
  feedback.value = null
  showAnswer.value = false
  guess.value = ''
  const res = await fetch('http://localhost:8000/random-set')
  set.value = await res.json()
  isLoading.value = false
}

async function submitGuess() {
  if (!set.value) return
  const res = await fetch('http://localhost:8000/guess', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ guess: guess.value, answer: set.value.name }),
  })
  const data = await res.json()
  feedback.value = data.correct ? 'correct' : 'wrong'
}

onMounted(fetchSet)
</script>

<template>
  <div class="container">
    <h1>🧩 LEGO Set Guessing Game</h1>
    <h2>Guess the LEGO Set</h2>

    <div v-if="isLoading">Loading...</div>

    <div v-else-if="set">
      <div style="display: flex; flex-direction: row;">
 <img :src="set.image_url" class="lego-image" alt="LEGO Set" />

      <div class="meta">
        <p><strong>Theme:</strong> {{ set.theme }}</p>
        <p><strong>Year:</strong> {{ set.year }}</p>
        <p><strong>Pieces:</strong> {{ set.pieces }}</p>
      </div>
      </div>
     

      <form @submit.prevent="submitGuess" class="guess-form">
        <input class="input-enter-guess" v-model="guess" placeholder="Enter your guess" />
        <button class="btn-submit" type="submit">Check</button>
      </form>

      <div v-if="feedback === 'correct'" class="feedback correct">✅ Correct!</div>
      <div v-else-if="feedback === 'wrong'" class="feedback wrong">❌ Try again!</div>

      <div class="grid-buttons">
        <button @click="showAnswer = !showAnswer" class="reveal-btn">
          {{ showAnswer ? 'Hide' : 'Show' }} Answer
        </button>

        <button @click="fetchSet" class="new-btn">
          🔄 New Set
        </button>
      </div>

      <div v-if="showAnswer" class="answer">📦 Answer: {{ set.name }}</div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 24px;
  background: white;
  border-radius: 20px;
  border: 1px solid #f7d117;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  text-align: center;
  font-family: monospace;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}

h1 {
  color: #da291c;
  font-size: 1.8rem;
  margin: 0;
}

h2 {
  color: #333;
  font-size: 1.2rem;
  margin: 0;
}

.lego-image {
  max-width: 240px;
  border-radius: 6px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}

.meta p {
  margin: 0.25rem 0;
}

.guess-form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
  width: 360px;
}

.input-enter-guess {
  padding: 12px 20px;
  font-size: 1rem;
  border-radius: 10px;
  border: 1px solid #ccc;
  flex: 1 1 auto;
  height: 24px;
  font-family: monospace;
  min-width: 200px;
}

.btn-submit {
  background-color: #f7d117;
  color: white;
  font-weight: bold;
  border: none;
  padding: 0 24px;
  border-radius: 999px;
  width: 156px;
  cursor: pointer;
  height: 48px;
  font-family: monospace;
  font-size: 1rem;
  transition: background 0.3s;
}

.btn-submit:hover {
  background-color: #ffe133;
}

.feedback {
  font-size: 1.2rem;
  margin: 0.5rem 0;
}

.correct {
  color: green;
}

.wrong {
  color: red;
}

.grid-buttons {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 8px;
}

.reveal-btn,
.new-btn {
  background-color: #0055bf;
  color: white;
  padding: 12px 20px;
  border-radius: 999px;
  border: none;
  font-weight: bold;
  cursor: pointer;
  font-family: monospace;
  transition: background 0.3s;
  font-size: 1rem;
  height: 48px;
}

.reveal-btn:hover,
.new-btn:hover {
  background-color: #003f94;
}

.answer {
  font-style: italic;
  color: #333;
  margin-top: 6px;
}
</style>
