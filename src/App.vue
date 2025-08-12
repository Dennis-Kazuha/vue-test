<template>
  <main style="max-width: 640px; margin: 32px auto; font-family: system-ui;">
    <h1>Playground</h1>
    <h2>{{ title }}</h2>
    <p :class="{hot: count>= 10}">Count:{{ count }}</p>
    <button @click="inc()">+1</button>
    <button @click="reset()">Reset</button>
    <button @click="dec()">-1</button>

  <section style="margin-top: 16px;">
    <h3>Adder</h3>
    <input v-model.number="a" placeholder="a"/>
    <input v-model.number="b" placeholder="b"/>
    <p>Sum: {{ sum }}</p>
    <p>{{ title }}</p>
  </section>
  </main>
</template>

<script setup>
const title ='My first page'

import { ref } from 'vue';
const count = ref(0)
const inc = () =>{
  count.value = count.value+1
}
const reset = () => {
  count.value = 0
}
const dec = () => { if (count.value > 0) count.value--}

import { computed } from 'vue'

const a = ref(0)
const b = ref(0)
const sum = computed(() =>a.value + b.value)

</script>

<style>
.hot {color: tomato;}

</style>