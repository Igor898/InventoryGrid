<script setup>
import { ref } from 'vue';
import ItemDetails from './ItemDetails.vue';

defineProps({ item: Object });
const showDetails = ref(false);
</script>

<template>
  <div class="item" :style="{ backgroundColor: item.color }" @click="showDetails = true">
    <span>{{ item.name }}</span>
    <ItemDetails
      v-if="showDetails"
      :item="item"
      @close="showDetails = false"
      @remove="$emit('remove', item.id)"
    />
  </div>
</template>

<style scoped>
.item {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 12px;
  cursor: pointer;
  border-radius: 5px;
  position: relative;
}
</style>
