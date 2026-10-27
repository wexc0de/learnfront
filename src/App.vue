<script setup>
import { ref, computed } from 'vue'
import ItemList from './ItemList.vue'

const items = ref([
  { id: 1, text: 'Piim', isDone: false, priority: 'high' },
  { id: 2, text: 'Leib', isDone: true, priority: 'low' },
  { id: 3, text: 'Munad', isDone: false, priority: 'medium' },
  { id: 4, text: 'Juust', isDone: false, priority: 'high' }
])

const newItem = ref('')
const newPriority = ref('medium')

const priorityOrder = {
  high: 3,
  medium: 2,
  low: 1
}

const sortedItems = computed(() => {
  return [...items.value].sort(
    (a, b) => priorityOrder[b.priority] - priorityOrder[a.priority]
  )
})

const doneItems = computed(() => {
  return sortedItems.value.filter(item => item.isDone)
})

const toDoItems = computed(() => {
  return sortedItems.value.filter(item => !item.isDone)
})

function addItem() {
  if (newItem.value.trim() === '') return

  items.value.push({
    id: Date.now(),
    text: newItem.value.trim(),
    isDone: false,
    priority: newPriority.value
  })

  newItem.value = ''
  newPriority.value = 'medium'
}

function toggleItem(id) {
  const item = items.value.find(item => item.id === id)
  if (item) {
    item.isDone = !item.isDone
  }
}
</script>

<template>
  <div class="page">
    <h1 class="main-title">Ostunimekiri</h1>

    <form class="form-box" @submit.prevent="addItem">
      <div class="form-group">
        <label>Uus asi</label>
        <input
          v-model="newItem"
          type="text"
          placeholder="Sisesta asja nimi"
        />
      </div>

      <div class="form-group">
        <label>Olulisus</label>
        <select v-model="newPriority">
          <option value="high">Kõrge</option>
          <option value="medium">Keskmine</option>
          <option value="low">Madal</option>
        </select>
      </div>

      <button type="submit">Lisa asi</button>
    </form>

    <div class="lists">
      <ItemList
        title="All items"
        :items="sortedItems"
        @toggle="toggleItem"
      />

      <ItemList
        title="Done items"
        :items="doneItems"
        @toggle="toggleItem"
      />

      <ItemList
        title="ToDo items"
        :items="toDoItems"
        @toggle="toggleItem"
      />
    </div>
  </div>
</template>


<style scoped>
.page {
  margin: 25px;
  color: black;
}

.main-title {
  font-size: 27px;
  font-weight: bold;
  margin-bottom: 20px;
}

.form-box {
  border: 1px solid #aaa;
  padding: 12px;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 12px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input,
.form-group select {
  padding: 6px;
  border: 1px solid #aaa;
  background: white;
  color: black;
}

button {
  padding: 6px 12px;
  background: #ddd;
  border: 1px solid #888;
  color: black;
  cursor: pointer;
}

.lists {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.lists > * {
  flex: 1;
  min-width: 220px;
}
</style>
