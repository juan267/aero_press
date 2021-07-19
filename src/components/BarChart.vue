<script lang="ts" setup>
import {
  Chart,
  BarElement,
  BarController,
  CategoryScale,
  LinearScale,
  Legend,
  Title,
  Tooltip,
} from 'chart.js'

Chart.register(
  BarElement,
  BarController,
  CategoryScale,
  LinearScale,
  Legend,
  Title,
  Tooltip,
)

const props = defineProps({
  title: {
    type: String,
  },
  labels: {
    type: Array,
    required: true,
  },
  data: {
    type: Array,
    required: true,
  },
})

const barChart = ref(null)

const chartData = {
  labels: props.labels,
  datasets: [{
    label: 'Cantidad',
    data: props.data,
    backgroundColor: [
      'rgba(255, 99, 132, 0.2)',
      'rgba(255, 159, 64, 0.2)',
      'rgba(255, 205, 86, 0.2)',
      'rgba(75, 192, 192, 0.2)',
      'rgba(54, 162, 235, 0.2)',
      'rgba(153, 102, 255, 0.2)',
      'rgba(201, 203, 207, 0.2)',
    ],
    borderColor: [
      'rgb(255, 99, 132)',
      'rgb(255, 159, 64)',
      'rgb(255, 205, 86)',
      'rgb(75, 192, 192)',
      'rgb(54, 162, 235)',
      'rgb(153, 102, 255)',
      'rgb(201, 203, 207)',
    ],
    borderWidth: 1,
  }],
}

const chartConfig = {
  type: 'bar',
  data: chartData,
}

function buildChart() {
  nextTick(() => {
    new Chart(
      barChart.value,
      chartConfig,
    )
  })
}

onMounted(() => {
  buildChart()
})
</script>

<template>
  <div class="w-80 inline-block">
    <h3 class="text-lg leading-6 font-medium text-gray-900">
      {{ title }}
    </h3>
    <canvas ref="barChart"></canvas>
  </div>
</template>
