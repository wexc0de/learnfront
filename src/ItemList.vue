<script setup>
defineProps({
  title: String,
  items: Array
})

defineEmits(['toggle'])

const priorityText = {
  high: 'Kõrge',
  medium: 'Keskmine',
  low: 'Madal'
}
</script>

<template>
  <div class="item-list">
    <h2 class="list-title">{{ title }}</h2>

    <div v-if="items.length === 0" class="empty-text">
      Pole ühtegi asja.
    </div>

    <div
      v-for="item in items"
      :key="item.id"
      class="item-row"
    >
      <label class="item-left">
        <input
          type="checkbox"
          :checked="item.isDone"
          @change="$emit('toggle', item.id)"
        />

        <span :class="{ done: item.isDone }">
          {{ item.text }}
        </span>
      </label>

      <span
        class="priority-badge"
        :class="{
          high: item.priority === 'high',
          medium: item.priority === 'medium',
          low: item.priority === 'low'
        }"
      >
        {{ priorityText[item.priority] }}
      </span>
    </div>
  </div>
</template>


<style scoped>
.item-list {
  border: 1px solid #aaa;
  padding: 10px;
}

.list-title {
  font-size: 19px;
  font-weight: bold;
  margin-bottom: 10px;
}

.item-row {
  padding: 9px 3px;
  border-bottom: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  gap: 8px;
}

.item-left {
  display: flex;
  gap: 5px;
  align-items: center;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.priority-badge {
  padding: 2px 5px;
  font-size: 12px;
  color: black;
}

.high {
  background: #ffcccc;
}

.medium {
  background: #fff0b3;
}

.low {
  background: #ccffcc;
}

.empty-text {
  color: gray;
}
</style>
