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

<div style="margin: 8px">
  <button @click="filter='all'" :class="{hot: filter==='all'}">ALL</button>
  <button @click="filter='active'" :class="{hot: fliter==='active'}">Active</button>
  <button @click="filter='done'" :class="{hot: filter==='done'}">Done</button>
</div>


  <section style="margin-top:16px;">
  <h3>Mini List</h3>

  <input v-model="newItem" placeholder="Add item" @keyup.enter="addItem()" />
  <button @click="addItem()" :disabled="!canAdd">Add</button>


  <p v-if="items.length === 0" style="opacity:.7; margin-top:6px;">(No items)</p>

  <ul style="margin-top:6px;">
    <li v-for="it in filterItems" :key="it.id" style="margin-top:6px;">
      <input type="checkbox" :checked="it.done" @change="toggleDone(it.id)" />
      <span :class="{ done: it.done }">{{ it.text }}</span>
      <button @click="removeItem(it.id)">x</button>
    </li>
    <footer style="margin-top: 8px;display: flex; gap:12px; align-items: center;">
      <span>{{ leftCount }}item left</span>
      <button @click="clearComplted()":disable="items.every(it=> !it.done)">
       Clear complted
      </button>
    </footer>

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
import { ref, computed, watch, onMounted } from 'vue'   // 合併 import 放最上面

const filter = ref('all')
const filteredItems = computed(() =>{
  if (filter.value === 'active') return items.value.filter(it=> !it.done)
  if (filter.value ==='done')    return items.value.filter(it=> !it.done)
return items.value
})

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

const a = ref(0)
const b = ref(0)
const sum = computed(() =>a.value + b.value)

const leftCount = computed(()=> items.value.filter(it=> !it.done).length)
const clearComplted = () => {
  items.value = items.value.filter(it=> !it.done)
}

const STORAGE_KEY = 'todo-items-v1'

// 載入
onMounted(() => {
  const raw = localStorage.getItem(STORAGE_KEY)
  if (!raw) return
  try {
    const parsed = JSON.parse(raw)
    if (Array.isArray(parsed)) {
      items.value = parsed.map(it => ({
        id: it.id ?? Date.now() + Math.random(),
        text: String(it.text ?? ''),
        done: !!it.done
      }))
    }
  } catch (e) {
    console.warn('Failed to load todos:', e)
  }
})

// 監看並存檔（deep 讓內部屬性變更也會觸發）
watch(items, (val) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(val))
}, { deep: true })

const canAdd = computed(() => newItem.value.trim().length > 0)


</script>

<style>
.hot {color: tomato;}
section{line-height: 1.6;}
.done { text-decoration: line-through; opacity: .6; }
li button { margin-left: 8px; }
input { margin-right: 8px; }
.done { text-decoration: line-through; opacity: .6; }



</style>