<script setup>
import {ref} from "vue";

const zodiacIntervals = {
  'Водолей': ['21.01', '18.02'],
  'Рыбы': ['19.02', '20.03'],
  'Овен': ['21.03', '19.04'],
  'Телец': ['20.04', '20.05'],
  'Близнецы': ['21.05', '21.06'],
  'Рак': ['22.06', '22.07'],
  'Лев': ['23.07', '22.08'],
  'Дева': ['23.08', '22.09'],
  'Весы': ['23.09', '23.10'],
  'Скорпион': ['24.10', '22.11'],
  'Стрелец': ['23.11', '21.12'],
  'Козерог': ['22.12', '20.01']
};
const zodiacKeys = Object.keys(zodiacIntervals);
const zodiacValues = Object.values(zodiacIntervals);
const hashZodiac = zodiacValues.reduce((res, date, index) => { //решил сделать хеш таблицу по которой буду определять начало и конец периода знака зодиака
  const partsStart = date[0].split('.');
  const dayStart = partsStart[0]
  const monthStart = partsStart[1]
  const partsEnd = date[1].split('.');
  const dayEnd = partsEnd[0]
  const monthEnd = partsEnd[1]

  res.start[monthStart] = {
    day: dayStart,
    zodiacName: zodiacKeys[index],
    previousZodiacName: zodiacKeys[index - 1 > 0 ? index - 1 : zodiacKeys.length - 1]
  };

  res.end[monthEnd] = {
    day: dayEnd,
    zodiacName: zodiacKeys[index],
    nextZodiacName: zodiacKeys[index + 1 > zodiacKeys.length - 1 ? 0 : index + 1]
  };

  return res;
}, {start: {}, end: {}})
const date = ref('');
const zodiac = ref('');
const arrResults = ref([]);
const error = ref(undefined);
const checkZodiac = () => {

  const parseDate = date.value.split('.')
  if(parseDate.length<3){
    error.value = "Неверный формат данных. Пример: 26.11.1994"
    return
  }
  const day = parseDate[0];
  const month = parseDate[1];
  const year = parseDate[1];
  const start = hashZodiac.start[month];
  const end = hashZodiac.end[month];

  const checkDate = new Date(parseInt(year), parseInt(month) - 1, parseInt(day)); // Месяц начинается с 0, поэтому вычитаем 1
  if (isNaN(checkDate)) {
    error.value = 'Дата некорректна';
    return
  } else if (checkDate.getMonth() !== parseInt(month) - 1 || checkDate.getDate() !== parseInt(day)) {
    error.value = 'Дата некорректна';
    return
  }

  if (start.day <= day) {
    arrResults.value.unshift({zodiacName:start.zodiacName, date: date.value})
    return
  }
  if (end.day >= day) {
    arrResults.value.unshift({zodiacName:end.zodiacName, date: date.value})
    return
  }
}

const resetError = () => {
  error.value = '';
}
/* For test
const newDate = new Date('01.01.2024');
for(let i = 0; i<366; i++){
  newDate.setDate(newDate.getDate()+1);
  const checkDate = `${(newDate.getDate()<10?'0':'')+newDate.getDate()}.${(newDate.getMonth()+1<10?'0':'')+(newDate.getMonth()+1)}.${newDate.getFullYear()}`;
  console.log(checkZodiac(checkDate)+" - "+checkDate);
}*/

</script>

<template>
  <div class="zodiac-error" v-if="error">{{error}}</div>
  <input class="zodiac-input" type="text" v-model="date" v-on:input="resetError">
  <div class="zodiac-button" v-on:click="checkZodiac">Узнать какой зодиак</div>
  <div class="zodiac-result" v-for="result in arrResults">{{result.date}} => {{result.zodiacName}}</div>
</template>

<style scoped>
.zodiac-error {
  color: red
}
.zodiac-input{
  margin-top: 1vh;
}
.zodiac-button{
  background: darkgrey;
  width: max-content;
  cursor: pointer;
  margin-top: 1vh;
}
.zodiac-button:hover{
  background: beige;
}
</style>