<script setup>
import { ref } from "vue";

const items = ref([
  "elemento1",
  "elemento2",
  "elemento3",
  "elemento4",
  "elemento5",
]);

const draggedItem = ref(null);

const handleDragStart = (index) => {
  draggedItem.value = index;
};

const handleDragOver = (event) => {
  event.preventDefault();
};

const handleDrop = (index) => {
  const droppedItem = items.value.splice(draggedItem.value, 1)[0];
  items.value.splice(index, 0, droppedItem);
};

const handleDragEnd = () => {
  draggedItem.value = null;
};
</script>

<template>
  <div class="sortable-list">
    <div
      v-for="(item, index) in items"
      :key="index"
      :draggable="true"
      @dragstart="handleDragStart(index)"
      @dragover="handleDragOver"
      @drop="handleDrop(index)"
      @dragend="handleDragEnd"
      :class="{ dragged: draggedItem === index }"
    >
      {{ item }}
    </div>
  </div>
</template>

<style scoped>
.sortable-list {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.sortable-list div {
  padding: 20px;
  margin-bottom: 5px;
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  cursor: move;
  transition: background-color 0.2s ease-in-out;
}

.sortable-list div.dragged {
  background-color: blue;
}
</style>
