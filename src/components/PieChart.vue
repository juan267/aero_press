<script lang="ts" setup>
import {
  Chart,
  ArcElement,
  DoughnutController,
  PieController,
  Legend,
  Title,
  Tooltip,
} from 'chart.js'
import ChartDataLabels from 'chartjs-plugin-datalabels'

Chart.register(
  ChartDataLabels,
  ArcElement,
  DoughnutController,
  PieController,
  Legend,
  Title,
  Tooltip,
)

const CHART_COLORS = {
  grey: 'rgb(201, 203, 207)',
  red: 'rgb(255, 99, 132)',
  yellow: 'rgb(255, 205, 86)',
  purple: 'rgb(153, 102, 255)',
  green: 'rgb(75, 192, 192)',
  orange: 'rgb(255, 159, 64)',
  blue: 'rgb(54, 162, 235)',
}

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
  type: {
    type: String,
    default: 'Porcentaje',
  },
})

const pieChart = ref(null)

const chartData = {
  labels: props.labels,
  datasets: [{
    label: props.type,
    data: props.data,
    backgroundColor: Object.values(CHART_COLORS),
    hoverOffset: 4,
  }],
}

const chartConfig = {
  type: 'doughnut',
  data: chartData,
  options: {
    plugins: {
      tooltip: {
        callbacks: {
          label(context) {
            return props.type === 'Porcentaje'
              ? `${context.dataset.label}: ${context.formattedValue}%`
              : `${context.dataset.label}: ${context.formattedValue}`
          },
        },
      },
    },
  },
}

function buildChart() {
  new Chart(
    pieChart.value,
    chartConfig,
  )
}

onMounted(() => {
  buildChart()
})
</script>

<template>
  <div class="w-65 inline-block">
    <h3 class="text-lg leading-6 font-medium text-gray-900">
      {{ title }}
    </h3>
    <canvas ref="pieChart"></canvas>
  </div>
</template>
