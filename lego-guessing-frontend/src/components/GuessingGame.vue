<script setup lang="ts">
import { ref, onMounted } from 'vue'
import backgroundImage from '../assets/blocks-background.png'

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
<div class="container" :style="{ backgroundImage: `url(${backgroundImage})` }">
<div class="header-bar">
  <h1>LEGO SET GUESSING GAME</h1>
</div>


    <div v-if="isLoading">Loading...</div>

    <div v-else-if="set">
      <div class="image-meta-wrapper">
        <img :src="set.image_url" class="lego-image" alt="LEGO Set" />
        <div class="meta">
          <p><strong>Theme:</strong> {{ set.theme }}</p>
          <p><strong>Year:</strong> {{ set.year }}</p>
          <p><strong>Pieces:</strong> {{ set.pieces }}</p>
        </div>
      </div>

      <form @submit.prevent="submitGuess" class="guess-form">
        <input class="input-enter-guess" v-model="guess" placeholder="Enter your guess" />
        <button class="button yellow" style="color: white;" type="submit">CHECK</button>
      </form>

      <div v-if="feedback === 'correct'" class="feedback correct">Correct!</div>
      <div v-else-if="feedback === 'wrong'" class="feedback wrong">Try again!</div>

      <div class="grid-buttons">
        <button @click="showAnswer = !showAnswer" class="button">
          {{ showAnswer ? 'HIDE' : 'SHOW' }} ANSWER
        </button>
        <button @click="fetchSet" class="button">NEW SET</button>
      </div>
      <div v-if="showAnswer" class="answer">Answer: {{ set.name }}</div>
    </div>
  </div>
</template>

<style scoped>
@media (min-width: 600px) {
  .image-meta-wrapper {
    /* border-left: 1px solid #eee; */
    /* padding-left: 20px; */
  }
}

.container {
  max-width: 800px;
  width: 90%;
  min-height: 700px;
  margin: 40px auto;
  padding: 24px;
  /* background-image: url('@/assets/blocks-background.jpg'); */
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  border-radius: 16px;
  /* border: 1px solid 	#FFD700; */
  box-shadow: 0 8px 24px rgba(255, 234, 0, 0.08);
  text-align: center;
  font-family: "Rubik", sans-serif;
  font-weight: 100;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}
.feedback,
.answer {
  transition: all 0.3s ease;
}
.header-bar {
  background-color: #FFD700;
  width: 100%;
  margin-top: -24px; /* cancel container's padding */
  margin-bottom: 8px;
  padding: 24px;
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;
  border-bottom: 2px solid -var(--lego-red);
}

.header-bar h1 {
  margin: 0;
  color: white;
  font-family: "Rubik", monospace;
  font-weight: 100;
  font-size: 1.4rem;
  letter-spacing: 0.06cap;
}

.divider {
  margin-top: 8px;
  height: 4px;
  width: 80px;
  background-color: #da291c; /* LEGO Red */
  margin-left: auto;
  margin-right: auto;
  border-radius: 4px;
}


h1 {
  color: 	#FFD700;
  font-size: 1.8rem;
  margin: 0;
  font-weight: 100;
  font-family: "Rubik", monospace;
}

h2 {
  color: #333;
  font-size: 1.2rem;
  margin: 0;
  font-weight: 100;
  font-family: "Rubik", sans-serif;
}

.image-meta-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
  flex-wrap: wrap;
  width: 100%;
  max-width: 500px;
  margin-bottom: 24px;
}

.lego-image {
 max-width: 200px;
  max-height: 200px;
  width: auto;
  height: auto;
  object-fit: contain;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  flex-shrink: 0;
}

.meta {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10px;
  font-family: sans-serif;
  font-weight: 300;
  font-size: 1rem;
  line-height: 1.4;
  color: #333;
  padding-left: 4px;
}
.meta p {
  margin: 0;
  text-align: left;
}
.meta p strong {
  display: inline-block;
  min-width: 70px;
  color: #201d48;
  font-weight: 500;
}

.guess-form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
  width: 100%;
  max-width: 380px;
  margin: 0 auto;
}

.input-enter-guess {
  padding: 12px 20px;
  font-size: 1rem;
  border-radius: 12px;
  border: 1px solid #ccc;
  font-family: "Rubik", sans-serif;
  font-weight: 100;
  flex: 1 1 auto;
  min-width: 200px;
  height: 48px;
  box-sizing: border-box;
}

.button {
  background-color: #201d48;
  color: white;
  padding: 12px 20px;
  border-radius: 28px;
  border: none;
  font-size: 1rem;
  font-family: "Rubik", sans-serif;
  font-weight: 100;
  cursor: pointer;
  transition: background 0.3s;
  width: 100%;
  max-width: 180px;
  height: 48px;
  box-sizing: border-box;
  display: inline-flex;
  justify-content: center;
  align-items: center;
}

.button:hover {
  background-color: #003f94;
}

.button.yellow {
  background-color: 	#FFD700;
  color: black;
  width: 100%;
  /* max-width: 180px; */
  height: 48px;
  border-radius: 28px;
  transition: background 0.3s;
}

@media (min-width: 600px) {
  .button.yellow {
    max-width: 100%;
  }
}

.button.yellow:hover {
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
  width: 100%;
  max-width: 380px;
  margin: 0 auto;
}

.answer {
  font-style: italic;
  color: #333;
  margin-top: 6px;
  font-family: "Rubik", sans-serif;
  font-weight: 100;
}

@media (max-width: 600px) {
  .image-meta-wrapper {
    flex-direction: column;
    align-items: center;
  }

  .guess-form,
  .grid-buttons {
    flex-direction: column;
    align-items: center;
  }

  .input-enter-guess,
  .button {
    max-width: 100%;
  }

  .meta {
    text-align: center;
  }
}
</style>
