<template>
  <div
    class="min-h-screen bg-gradient-to-tr from-green-400 to-blue-500 flex justify-center items-center px-4 py-8"
  >
    <div
      v-motion
      :initial="{ opacity: 0, y: 30 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 0.6 } }"
      class="bg-white rounded-xl shadow-xl w-full max-w-xl p-8 text-center space-y-6"
    >
      <h1 class="text-3xl font-bold text-gray-800">Κουίζ Ποδοσφαίρου</h1>
      <p class="text-gray-600">Μπορείς να βρεις τον ποδοσφαιριστή από την καριέρα του;</p>
      <p class="text-xl underline font-semibold">Πώς παίζεται</p>
      <div class="text-left">
        <ol class="list-decimal text-left text-sm text-gray-700 space-y-2 pl-6">
          <li>Θα δεις τις ομάδες και τις χρονιές στις οποίες αγωνίστηκε κάθε ποδοσφαιριστής.</li>
          <li>
            Πληκτρολόγησε το όνομά του στα Ελληνικά ή Αγγλικά. Δεν λαμβάνονται υπόψη τόνοι,
            πεζά/κεφαλαία ή σημεία στίξης.<br />
            Π.χ. <span class="italic">Frank Lampard</span> →
            <span class="font-semibold">φρανκ λαμπαρντ</span> ή
            <span class="font-semibold">frank lampard</span> ή
            <span class="font-semibold">ΦΡΑΝΚ ΛΑΜΠΑΡΝΤ</span> .Τα ονόματα είναι ίδια με αυτά του
            wikipedia
          </li>
          <li>
            Μπορείς να δώσεις ως απάντηση ακόμα και το επίθετο ενώς παίκτη ή το όνομα που είχε στην
            φανέλα<br />
            Π.χ. <span class="italic">Frank Lampard</span> →
            <span class="font-semibold">λαμπαρντ</span> ή <span class="font-semibold">lampard</span>
          </li>
          <li>
            Έχεις στη διάθεσή σου <span class="font-bold">2 λεπτά</span> για να απαντήσεις όσες
            περισσότερες ερωτήσεις μπορείς.
          </li>
          <li>
            Κάθε σωστή απάντηση δίνει από <span class="font-bold">1 έως 10 πόντους</span> ανάλογα με
            τη δυσκολία.
          </li>
          <li>
            Οι πόντοι κάθε ερώτησης αναγράφονται
            <span class="text-sm bg-cyan-300 font-bold rounded-md text-gray-800 px-3 py-1 mb-4"
              >Πόντοι:5</span
            >
          </li>
          <li>
            Μπορεις να πάρεις βοήθεια με το κουμπί
            <span class="bg-green-400 text-sm text-white px-2 py-1 rounded-md cursor-pointer"
              >💡 Πάρε Βοήθεια</span
            >
            αλλα θα μειώσεις τους πόντους της ερώτησης κατα -2
          </li>
        </ol>
      </div>

      <button
        v-motion
        :initial="{ scale: 1 }"
        :hovered="{ scale: 1.15, transition: { duration: 0.1 } }"
        @click="startQuiz"
        class="bg-blue-500 text-white px-6 py-3 rounded-lg text-lg font-semibold cursor-pointer transition"
      >
        Παίξε το Κουίζ
      </button>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()
function startQuiz() {
  router.push('/quiz')
}
</script>
