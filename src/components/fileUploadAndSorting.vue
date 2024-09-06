<script setup>
  import {ref} from "vue";

  const fileProcessing = () => {
    const fileInput = document.getElementById('fileFilter');
    const file = fileInput.files[0];
    const xhr = new XMLHttpRequest(); // Создаем объект XMLHttpRequest
    const formData = new FormData(); // Создаем объект FormData

    formData.append('fileFilter', file);
    xhr.onload = function() {
      let res = JSON.parse(xhr.response);
      downloadSrc.value = res.downloadPath
      console.log({res});
    };
    xhr.onerror = function() {
      console.error('Ошибка:', this.error);
    };
    xhr.open('POST', '/upload'); // Устанавливаем метод и URL сервера
    xhr.send(formData); // Отправляем запрос на сервер
  }
  const downloadSrc = ref(undefined);
</script>

<template>
  <div>
    <input type="file" id="fileFilter" name="fileFilter"><br><br>
    <div class="file-button" v-on:click="fileProcessing">Отправить</div>
    <div v-if="downloadSrc"><a :href="downloadSrc">Скачать отфильтрованный файл</a></div>
  </div>
</template>

<style scoped>

.file-button{
  background: darkgrey;
  width: max-content;
  cursor: pointer;
  margin-top: 1vh;
}
.file-button:hover{
  background: beige;
}
</style>