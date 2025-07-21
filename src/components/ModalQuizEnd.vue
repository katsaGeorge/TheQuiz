<template>
  <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-md text-center">
    <h2 class="text-2xl font-bold mb-4">🎉 Τέλος κουίζ!</h2>
    <p class="text-lg mb-4">Σκορ: {{ score }}</p>

        <div class="flex flex-col items-center  space-y-2">
        <button
            @click="PlayAgain"
            class="bg-green-500 hover:bg-green-600 text-white py-2 px-6 rounded-md cursor-pointer"
        >
            Παίξε Ξανά
        </button>

        <button
            @click="HomePage"
            class="bg-blue-500 hover:bg-blue-600 text-white py-2 px-6 rounded-md cursor-pointer"
        >
            Αρχική Σελίδα
        </button>
        </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
const emit = defineEmits(['play-again', 'home-page'])
const router = useRouter()
const props = defineProps({
  score: {
    type: Number,
    required: true
  },
  questions: {
    type: Array,
    required: true
  }
})

function PlayAgain() {
  emit('play-again')
  
}

function HomePage() {
  emit('home-page')
  
}

</script>