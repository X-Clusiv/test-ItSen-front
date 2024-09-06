<script setup>
import AngleBetweenArrows from "@/components/angleBetweenArrows.vue";
import {ref} from "vue";
import FileUploadAndSorting from "@/components/fileUploadAndSorting.vue";
import Zodiac from "@/components/zodiac.vue";

const pages = {
  angle: {id: "angle", title: "Задание 1 угол между часовой и стрелочной стрелкой"},
  file: {id: "file", title: "Задание 2 Загрузка файла и сортировка"},
  zodiac: {id:"zodiac", title: "Задание 3 Зодиак"}
}

const page = ref(undefined);

const isActive = (pageName) => page.value === pageName ? 'active' : '';
const goToPage = (pageName) => page.value = pageName;

</script>

<template>
  <div class="menu">
    <div class="menu-item" :class="isActive(page.id)" v-on:click="goToPage(page.id)" v-for="page in pages">
      {{ page.title }}
    </div>
  </div>
  <angle-between-arrows v-if="page === pages.angle.id"/>
  <file-upload-and-sorting v-if="page === pages.file.id"/>
  <zodiac v-if="page === pages.zodiac.id"/>
</template>

<style scoped>
body {
  padding: 0;
  margin: 0;
}

.menu {
  margin-bottom: 1vh;
}

.menu-item {
  cursor: pointer;
  background: cadetblue;
  width: max-content;
  margin-bottom: 1vh;
}

.menu-item.active {
  background: aqua;
}

.menu-item:hover {
  background: aquamarine;

}
</style>
