<template>
  <div class="inventory">
    <div class="grid">
      <div
        v-for="(item, index) in inventory"
        :key="index"
        class="cell"
        @dragover.prevent
        @drop="moveItem(index)"
      >
        <div
          v-if="item"
          class="item"
          :class="item.shape"
          :style="{ backgroundColor: item.color }"
          draggable="true"
          @dragstart="startDrag(index)"
          @click="selectItem(index)"
        >
          {{ item.name }}
        </div>
      </div>
    </div>

    <ItemDetails
      v-if="selectedItem !== null"
      :item="inventory[selectedItem]"
      @close="selectedItem = null"
      @delete="removeItem"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ItemDetails from './ItemDetails.vue';

const inventory = ref(Array(100).fill(null)); // 100 ячеек в инвентаре
const selectedItem = ref(null);
const draggedItem = ref(null);

// Загрузка инвентаря из localStorage
onMounted(() => {
  const savedInventory = localStorage.getItem('inventory');
  if (savedInventory) {
    inventory.value = JSON.parse(savedInventory);
  }
});

const startDrag = (index) => {
  draggedItem.value = index;
};

const moveItem = (targetIndex) => {
  if (draggedItem.value !== null && draggedItem.value !== targetIndex) {
    [inventory.value[targetIndex], inventory.value[draggedItem.value]] =
      [inventory.value[draggedItem.value], inventory.value[targetIndex]];
    draggedItem.value = null;
    saveInventory();
  }
};

const selectItem = (index) => {
  selectedItem.value = index;
};

const removeItem = () => {
  if (selectedItem.value !== null) {
    inventory.value[selectedItem.value] = null;
    saveInventory();
    selectedItem.value = null;
  }
};

// Добавление нового предмета
const addItem = (item) => {
  const emptyIndex = inventory.value.findIndex((cell) => cell === null);
  if (emptyIndex !== -1) {
    inventory.value[emptyIndex] = item;
    saveInventory();
  }
};

// Сохранение инвентаря
const saveInventory = () => {
  localStorage.setItem('inventory', JSON.stringify(inventory.value));
};

// Экспортируем методы
defineExpose({ addItem });
</script>

<style scoped>
.inventory {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.grid {
  display: grid;
  grid-template-columns: repeat(10, 80px);
  gap: 10px;
}

.cell {
  width: 80px;
  height: 80px;
  background-color: #222;
  border: 1px solid #555;
  display: flex;
  align-items: center;
  justify-content: center;
}

.item {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 12px;
  cursor: grab;
  position: relative;
  border-radius: 5px;
}

.square {
  border-radius: 0;
}

.circle {
  border-radius: 50%;
}

.rectangle {
  width: 80px;
  height: 40px;
}

.triangle {
  width: 0;
  height: 0;
  border-left: 30px solid transparent;
  border-right: 30px solid transparent;
  border-bottom: 60px solid;
}
</style>
