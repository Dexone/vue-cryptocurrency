<template>
  <div class="all">
    <h1>CRYPTOCURRENCY RATE</h1>


    <LineChart class="chart" :chartData="lineData" :options="options" />


    <div class="relevant">Data relevant to {{ time }}</div>
    <div class="update" @click="getValue">
      <img src="./images/update.png" width="43px">
    </div>



    <div class="lightwindow">
      <div v-for="people, index in peoples"><span class="buttSearch" @click="selected = peoples[index]"><img
            src="./images/search.png" width="17px"></span> {{ people }} <span @click="peoples.splice(index, 1)"> <img
            src="./images/bucket.png" width="15px" align="right"></span></div>
    </div>

    <div class="darkWindow">

      <input class="inp" v-model="inp">
      <button class="buttAdd" @click="addInput">+</button>
    </div>


<div class="made">
  Made with <a style="color: #4fc08d">Vue.js</a> by <a href="https://github.com/Dexone">Dexone</a>.
</div>




  </div>
</template>




<script setup>
import Chart from 'chart.js/auto'
import { LineChart } from "vue-chart-3"
import { ref, watch, computed, onMounted } from 'vue'
import axios from 'axios'
import { useStorage } from '@vueuse/core'

let selected = ref(["bitcoin"])
let inp = ref()
let time = ref()
function addInput() {
  peoples.value.push(inp.value)
  inp.value = ""
}




const peoples = ref(['bitcoin', 'ethereum', 'litecoin'])
const state = useStorage('vue-use-local-storage', peoples)

watch(selected, () => {
  getValue()
})
function getValue() {
  let Data = new Date()
  time.value = Data.toString().slice(16).slice(0, 8)
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

onMounted(() => {
  getValue()
})
</script>




