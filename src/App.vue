<template>
  <h1>playground</h1>

  <button @click="count++">+1</button>
  <button @click="reset()">Reset</button>
  <button @click="dec()">-1</button>

  <input v-model="name" placeholder="Your name" :class="{ error: nameTooLong }" />
  <p v-if="nameTooLong" class="error-text">最多 12個字</p>

  <p>Hi, {{ name || 'friend' }} 👋</p>

  <p :style="{ color: count >= 10 ? 'tomato' : '' }">Count: {{ count }}</p>
  <p>2x: {{ doubleCount }} ｜ squared: {{ squareCount }} ｜ parity: {{ parity }}</p>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const count = ref(5)
const name = ref('Kazuha')

const reset = () => { count.value = 0 }
const dec = () => { if (count.value > 0) count.value-- }

const doubleCount = computed(() => count.value * 2)
const squareCount = computed(() => count.value ** 2)
const parity = computed(() => (count.value % 2 === 0 ? 'even' : 'odd'))

const nameTooLong = computed(() => name.value.length > 12)

const hit10Once = ref(false)
watch(count, (now) => {
  if (!hit10Once.value && now === 10) {
    alert('Hit 10!')
    hit10Once.value = true
  }
})
</script>

<style>
.error { border: 1px solid tomato; }
.error-text { color: tomato; margin-top: 4px; }
button { margin-right: 8px; }
</style>
