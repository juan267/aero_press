<script lang="ts" setup>
import { Chart, LineController, CategoryScale, LinearScale, PointElement, LineElement } from 'chart.js'

Chart.register(LineController, CategoryScale, LinearScale, PointElement, LineElement)

defineProps({
  lotName: {
    type: String,
  },
  lot: {
    type: Object,
  },
})

const pieChart = ref(null)

const labels = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
]

const chartData = {
  labels,
  datasets: [{
    label: 'My First dataset',
    backgroundColor: 'rgb(255, 99, 132)',
    borderColor: 'rgb(255, 99, 132)',
    data: [0, 10, 5, 2, 20, 30, 45],
  }],
}

const config = {
  type: 'line',
  data: chartData,
  options: {},
}

function buildChart() {
  nextTick(() => {
    new Chart(
      pieChart.value,
      config,
    )
  })
}

onMounted(() => {
  buildChart()
})
</script>

<template>
  <div class="pb-5 border-b border-gray-200">
    <h3 class="text-lg leading-6 font-medium text-gray-900">
      {{ lotName }}
    </h3>
  </div>

  <div class="w-50">
    <canvas ref="pieChart"></canvas>
  </div>
</template>
