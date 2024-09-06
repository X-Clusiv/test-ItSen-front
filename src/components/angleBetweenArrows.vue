<script setup>
import {ref} from "vue";

const angle = ref(undefined);
const time = ref("");
const error = ref(undefined);
const errorHandler = () => {
  if (!time.value) {
    error.value = "Укажите время в формате чч:мм (23:00)"
    return true
  }
  const dataTime = time.value.split(':');
  if (dataTime.length !== 2 || isNaN(parseInt(dataTime[0])) || isNaN(parseInt(dataTime[1]))) {
    error.value = "Неверный формат данных. Пример: 22:00";
    return true
  }
  if (checkCorrectTime(dataTime)) {
    error.value = "Введено неверное время.";
    return true
  }
}
const checkCorrectTime = (dataTime) => dataTime[0] > 23 || dataTime[0] < 0 || dataTime[1] < 0 || dataTime[1] > 59 || dataTime[0].length > 2 || dataTime[1].length > 2
const resetError = () => {
  error.value = '';
}

function calculateAngle(event) {
  event.preventDefault();
  if (errorHandler()) {
    return
  }
  let [hh, mm] = time.value.split(":")

  hh = hh % 12;

  let h = (hh * 360) / 12 + (mm * 360) / (12 * 60);
  let m = (mm * 360) / 60;
  let result = Math.abs(h - m);

  if (result > 180) {
    result = 360 - result;
  }
  angle.value = result
}
</script>

<template>
  <div>
    <form id="angleBetweenArrows" enctype="multipart/form-data">
      <div class="arrows-error">{{ error }}</div>
      <input type="text" v-model="time" placeholder="23:00" v-on:input="resetError"/>
      <button class="arrows-button" v-on:click="calculateAngle">Высчитать угол</button>
    </form>
    <div v-if="angle !== undefined">{{ angle }}°</div>
  </div>
</template>

<style scoped>
.arrows-error {
  color: red
}

.arrows-button {
  cursor: pointer;
}
</style>