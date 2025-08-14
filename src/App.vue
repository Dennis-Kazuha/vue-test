<template>
  <main style="max-width: 640px; margin: 32px auto; font-family: system-ui;">
    <h1>Playground</h1>
    <h2>{{ title }}</h2>
    <p :class="{hot: count>= 10}">Count:{{ count }}</p>
    <button @click="inc()">+1</button>
    <button @click="reset()">Reset</button>
    <button @click="dec()">-1</button>
    <label style="display:block; margin-top: 16px;">
      <input type="checkbox" v-model="showAdvanced"/>
      Show advanced
    </label>

  <section style="margin-top: 16px;">
    <h3>Adder</h3>
    <input v-model.number="a" placeholder="a"/>
    <input v-model.number="b" placeholder="b"/>
    <p>Sum: {{ sum }}</p>
    <p>{{ title }}</p>
  </section>

  <section style="margin-top:16px;">
  <h3>Mini List</h3>

  <input v-model="newItem" placeholder="Add item" />
  <button @click="addItem()">Add</button>

  <p v-if="items.length === 0" style="opacity:.7; margin-top:6px;">(No items)</p>

  <ul style="margin-top:6px;">
    <li v-for="it in items" :key="it.id" style="margin-top:6px;">
      <input type="checkbox" :checked="it.done" @change="toggleDone(it.id)" />
      <span :class="{ done: it.done }">{{ it.text }}</span>
      <button @click="removeItem(it.id)">x</button>
    </li>

  </ul>
</section>


  <section v-if="showAdvanced" style="margin-top: 8px;">
    <p><strong>v-if:</strong></p>
  </section>

  <section v-show="showAdvanced" style="margin-top: 8px;">
    <p><strong>v-show:</strong></p>
  </section>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue'   // 合併 import 放最上面

const title = 'My first page'

const count = ref(0)
const inc = () =>{
  count.value = count.value+1
}
const reset = () => {
  count.value = 0
}
const dec = () => { if (count.value > 0) count.value--}
const showAdvanced = ref(false)
const items = ref([{ id:1, text: 'Learn ref',done:false}])
const newItem = ref('')
const addItem = () => {
  const t = newItem.value.trim()
  if (!t) return
  items.value.push({ id: Date.now(), text: t, done: false })  // items + Date.now()
  newItem.value = ''
}

const removeItem = (id) => {
  items.value = items.value.filter(it => it.id !== id)        // items（不是 item）
}

const toggleDone = (id) => {
  const it = items.value.find(x => x.id === id)
  if (it) it.done = !it.done
}


import { computed } from 'vue'

const a = ref(0)
const b = ref(0)
const sum = computed(() =>a.value + b.value)



</script>

<style>
.hot {color: tomato;}
section{line-height: 1.6;}
.done { text-decoration: line-through; opacity: .6; }
li button { margin-left: 8px; }
input { margin-right: 8px; }
.done { text-decoration: line-through; opacity: .6; }



</style>