<template>
  <div class="d-flex">
    <AddItem @add-item="addItemToInventory" />
    <InventoryGrid ref="inventoryGrid" />
  </div>
</template>

<script setup>
import InventoryGrid from './components/InventoryGrid.vue';
import AddItem from './components/AddItem.vue';
import { ref } from 'vue';

const inventoryGrid = ref(null);

const addItemToInventory = (item) => {
  // Используем метод addItem из InventoryGrid
  inventoryGrid.value.addItem(item);
};
</script>

<style scoped>
.d-flex{
  display: flex;
  justify-content: center;
}
</style>