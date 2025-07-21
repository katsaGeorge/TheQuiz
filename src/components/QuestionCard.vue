<template>
  <div
    class="min-h-screen bg-gradient-to-tr from-green-400 to-blue-500 flex justify-center items-center px-4 py-8"
  >
    <div class="bg-white rounded-xl shadow-xl w-full max-w-2xl p-6 space-y-6">
      <!-- Header -->
      <div>
        <h1 class="text-2xl font-bold text-gray-800 text-center mb-4">Βρες τον Ποδοσφαιριστή!</h1>
      </div>
      <div class="flex justify-between items-center text-gray-700 font-semibold text-sm">
        <div class="flex items-center gap-2">🏆 Score: {{ score }}</div>
        <div class="flex items-center gap-2">⏰ {{ timeLeft }}s</div>
      </div>

      <!-- Question -->
      <div>
        <h2 class="text-lg font-bold text-gray-800 mb-4">
          {{ currentQuestion.question }}
        </h2>
        <h6
          class="inline-block text-sm bg-cyan-300 font-bold rounded-md text-gray-800 px-3 py-1 mb-4"
        >
          Πόντοι: {{ currentPoints }}
        </h6>
        <ul class="space-y-2 text-sm text-gray-800">
          <li
            v-for="(line, i) in currentQuestion.career"
            :key="i"
            class="bg-gray-100 rounded-md px-4 py-2"
          >
            {{ line }}
          </li>
        </ul>
      </div>
      <div class="text-center">
        <button
          @click="toggleHint"
          class="bg-green-400 hover:bg-green-600 text-white py-2 px-6 rounded-md cursor-pointer"
        >
          💡 Πάρε Βοήθεια
        </button>

        <p v-if="showHint" class="mt-2 text-sm text-gray-600">
          {{ currentQuestion.hint }}
        </p>
      </div>
      <!-- Input -->
      <div>
        <input
          v-model="userAnswer"
          type="text"
          class="w-full border border-gray-300 rounded-md px-4 py-2 mt-4"
          placeholder="Η απάντησή σου..."
          @keyup.enter="submitAnswer"
        />
        <button
          @click="submitAnswer"
          :disabled="isSubmitDisabled"
          class="mt-3 w-full bg-blue-500 text-white py-2 rounded-md hover:bg-blue-600 transition cursor-pointer"
        >
          Υποβολή
        </button>
        <button
          @click="nextQuestion"
          class="mt-3 w-full bg-amber-400 text-white py-2 rounded-md hover:bg-amber-600 transition cursor-pointer"
        >
          Eπομένη Ερώτηση
        </button>
      </div>

      <!-- Feedback -->
      <div v-if="feedback" class="text-center text-md font-bold mt-4">
        {{ feedback }}
      </div>

      <!-- End of Quiz -->
      <div
        v-if="quizFinished"
        class="fixed inset-0 bg-black/50 flex items-center justify-center z-50"
      >
        <ModalQuizEnd
          :score="score"
          :questions="questions"
          @play-again="PlayAgain"
          @home-page="HomePage"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onBeforeUnmount, onMounted, watchEffect } from 'vue'
import questiondata from '@/assets/questionsdb.json'

import ModalQuizEnd from './ModalQuizEnd.vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const questions = ref(shuffleArray(questiondata.questions))

const currentIndex = ref(0)
const currentQuestion = computed(() => questions.value[currentIndex.value])
const isSubmitDisabled = computed(() => !userAnswer.value.trim())

const userAnswer = ref('')
const score = ref(0)
const quizFinished = ref(false)
const feedback = ref('')
const timeLeft = ref(150)
let timerInterval = null
const showHint = ref(false)

const currentPoints = ref(0)

watchEffect(() => {
  currentPoints.value = currentQuestion.value.points
})

onMounted(() => {
  timerInterval = setInterval(() => {
    if (timeLeft.value > 0) {
      timeLeft.value--
    } else {
      finishQuiz()
    }
  }, 1000)
})

onBeforeUnmount(() => {
  clearInterval(timerInterval)
})

function finishQuiz() {
  quizFinished.value = true
  clearInterval(timerInterval)
}
function normalizeAnswer(text) {
  return text
    .normalize('NFD') // σπάει τον χαρακτήρα και τον τόνο
    .replace(/[\u0300-\u036f]/g, '') // αφαιρεί τους τόνους
    .toLowerCase()
    .trim()
}

function submitAnswer() {
  if (quizFinished.value) return

  const user = normalizeAnswer(userAnswer.value)
  const correctAnswers = currentQuestion.value.correctAnswer.map(normalizeAnswer)
  if (correctAnswers.includes(user)) {
    feedback.value = '✅ Σωστά!'
    score.value = score.value + currentPoints.value
    showHint.value = false
  } else {
    feedback.value = `❌ Λάθος Απαντηση!`
  }

  setTimeout(() => {
    nextQuestion()
  }, 1200)
}

function PlayAgain() {
  currentIndex.value = 0
  score.value = 0
  quizFinished.value = false
  userAnswer.value = ''
  feedback.value = ''
  timeLeft.value = 150 // Επαναφορά του χρόνου σε 2 λεπτά
  showHint.value = false
  questions.value = shuffleArray(questions.value)
  clearInterval(timerInterval)

  timerInterval = setInterval(() => {
    if (timeLeft.value > 0) {
      timeLeft.value--
    } else {
      finishQuiz()
    }
  }, 1000)
}

function toggleHint() {
  showHint.value = true
  currentPoints.value -= 2
}

function HomePage() {
  router.push('/')
}
function nextQuestion() {
  userAnswer.value = ''
  feedback.value = ''
  if (currentIndex.value < questions.value.length - 1) {
    currentIndex.value++
    showHint.value = false
  } else {
    quizFinished.value = true
    clearInterval(timerInterval)
  }
}

function shuffleArray(array) {
  const shuffled = [...array]
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
  }
  return shuffled
}
</script>
