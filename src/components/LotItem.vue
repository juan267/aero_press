<script lang="ts" setup>
const props = defineProps({
  lotName: {
    type: String,
    required: true,
  },
  lot: {
    type: Object,
    required: true,
  },
})

const labels = [
  'blanco',
  'rojo',
  'tostador',
  'arañita',
]

const stats = [
  { name: 'Total Arboles', stat: props.lot.treeCount },
  { name: 'Total Frutos', stat: props.lot.fruitCount },
  { name: 'Promedio acaros X lupa', stat: props.lot.magnifyingAAverage },
]
</script>

<template>
  <div class="mt-10 bg-white overflow-hidden shadow rounded-lg">
    <div class="px-4 py-5 sm:p-6">
      <!-- Content goes here -->
      <div class="bg-white px-4 py-5 border-b border-gray-200 sm:px-6">
        <h3 class="text-left text-lg leading-6 font-medium text-gray-900">
          Lote: {{ lotName }}
        </h3>
      </div>

      <div>
        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <div v-for="item in stats" :key="item.name" class="px-4 py-5 bg-white shadow rounded-lg overflow-hidden sm:p-6">
            <dt class="text-sm font-medium text-gray-500 truncate">
              {{ item.name }}
            </dt>
            <dd class="mt-1 text-3xl font-semibold text-gray-900">
              {{ item.stat }}
            </dd>
          </div>
        </dl>
      </div>

      <div class="m-5 flex justify-between">
        <BarChart
          title="Cantidad plagas"
          :labels="labels"
          :data="[props.lot.whiteACount, props.lot.redACount, props.lot.toastACount, props.lot.littleSpiderCount]"
        />

        <PieChart
          title="Cantidad plagas"
          :labels="[...labels, 'frutos sanos']"
          :data="[props.lot.whiteACount, props.lot.redACount, props.lot.toastACount, props.lot.littleSpiderCount, props.lot.healthyFruitCount]"
        />

        <PieChart
          title="Porcentaje plagas"
          :labels="[...labels, 'frutos sanos']"
          :data="[props.lot.whiteAAverage, props.lot.redAAverage, props.lot.toastAAverage, props.lot.littleSpiderAverage, props.lot.healthyFruitAverage]"
        />
      </div>
    </div>
  </div>
</template>
