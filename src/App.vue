<template>
  <LineChart :chartData="lineData" :options="options" style="width:335px; height: 290px;" />


  <h2>CRYPTOCURRENCY RATE</h2>

  <select v-model="selected">
    <option v-for=" people in peoples">{{ people }}</option>
  </select>

  <input v-model="inp">
  <button @click="addInput">Добавить</button>
</template>




<script setup>
import Chart from 'chart.js/auto'
import { LineChart } from "vue-chart-3"
import { ref, watch, computed } from 'vue'
import axios from 'axios'

let inp = ref()
function addInput(){
peoples.value.push(inp.value)
}




const peoples = ref(['bitcoin', 'ethereum', 'litecoin'])
let selected = ref()

watch(selected, () => {
  getValue()
})
function getValue() {
  axios.get(`https://api.coincap.io/v2/assets/${selected.value}/history?interval=h1`).then((res) => {
    dataLabels.value = [res.data.data[715].date.slice(11, 16), res.data.data[716].date.slice(11, 16), res.data.data[717].date.slice(11, 16), res.data.data[718].date.slice(11, 16), res.data.data[719].date.slice(11, 16)]
    dataValues.value = [res.data.data[715].priceUsd, res.data.data[716].priceUsd, res.data.data[717].priceUsd, res.data.data[718].priceUsd, res.data.data[719].priceUsd]
  })
}
const dataLabels = ref([])
const dataValues = ref([])

const lineData = computed(() => ({
  labels: dataLabels.value,
  datasets: [
    {
      data: dataValues.value,
      backgroundColor: '#333639',
      borderColor: '#333639',
    },
  ],
}));
const options = {
  plugins: {
    legend: true
  }
}
</script>




