<script setup>
import { ref } from "vue";

const image = ref(null);
const dragging = ref(false);

const handleDragOver = (e) => {
  e.preventDefault();
};

const handleDragEnter = () => {
  dragging.value = true;
};
const handleDragLeave = () => {
  dragging.value = false;
};

const handleDrop = (e) => {
  e.preventDefault();
  const file = e.dataTransfer.files[0];
  handleImageUpload(file);
};

const handleImageUpload = (file) => {
  const reader = new FileReader();
  reader.onload = (e) => {
    image.value = e.target.result;
  };
  reader.readAsDataURL(file);
};
</script>

<template>
  <div
    class="drop-zone"
    @dragover="handleDragOver"
    @dragenter="handleDragEnter"
    @dragleave="handleDragLeave"
    @drop="handleDrop"
  >
    <div v-if="!image">
      {{ dragging ? "Suelta Aqui" : "Arrastra y suelta la imagen" }}
    </div>
    <img v-else :src="image" alt="" class="uploaded-image" />
  </div>
</template>

<style scoped>
.drop-zone {
  width: 300px;
  height: 300px;
  border: 2px dashed #ccc;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.drop-zone-dragging {
  border-color: #000;
}
</style>
