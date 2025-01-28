<template>
  <div class="add-item-container">
    <form @submit.prevent="submitForm">
      <div class="preview-container">
        <h1 class="text">Предпросмотр:</h1>
        <div
          class="preview-item"
          :class="itemShape"
          :style="{ backgroundColor: itemColor }"
        >
          {{ itemName || "Предмет" }}
        </div>
      </div>
      <label class="text">Название:</label>
      <input class="text" v-model="itemName" type="text" required />

      <label class="text">Цвет:</label>
      <input v-model="itemColor" type="color" required />

      <label class="text">Форма:</label>
      <select class="text" v-model="itemShape">
        <option class="text" value="square">Квадрат</option>
        <option class="text" value="circle">Круг</option>
        <option class="text" value="rectangle">Прямоугольник</option>
        <option class="text" value="triangle">Треугольник</option>
      </select>

      <!-- Превью предмета -->

      <button type="submit" class="text">Добавить предмет</button>
    </form>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";

const emit = defineEmits(["add-item"]);
const itemName = ref("");
const itemColor = ref("#3498db");
const itemShape = ref("square");

const submitForm = () => {
  const newItem = {
    id: Date.now(),
    name: itemName.value,
    color: itemColor.value,
    shape: itemShape.value,
  };
  emit("add-item", newItem);

  // Очищаем форму после отправки
  itemName.value = "";
  itemColor.value = "#3498db";
  itemShape.value = "square";
};
</script>

<style scoped>
.add-item-container {
  margin-bottom: 20px;
}
.text{
    color: white;
}
form {
  width: 400px;
  height: 880px;  
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  gap: 10px;
  background: #2f2e2e;
  padding: 15px;
  border-radius: 5px;
  align-items: center;
}

input,
select,
button {
  padding: 8px;
  font-size: 14px;
  width: 100%;
  background: #424242;
}

.preview-container {
  text-align: center;
}

.preview-item {
  width: 400px;
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 12px;
  cursor: pointer;
  position: relative;
  border-radius: 5px;
  text-align: center;
  margin: auto;
}

/* Стили форм */
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
