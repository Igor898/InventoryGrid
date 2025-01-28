<template>
  <div class="modal">
    <div class="modal-content">
      <h3>{{ item.name }}</h3>
      <p>{{ item.description }}</p>
      <button @click="$emit('close')">Закрыть</button>
      <button @click="$emit('delete')">Удалить</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['item']);
defineEmits(['close', 'delete']);
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  text-align: center;
}

button {
  margin: 5px;
  padding: 10px;
}
</style>
