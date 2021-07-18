<script setup lang="ts">
import { read, utils } from 'xlsx'
import { groupBy, countBy, sum } from 'ramda'

const data = ref([])
const SheetJSFT = [
  'xlsx', 'xlsb', 'xlsm', 'xls', 'xml', 'csv', 'txt', 'ods', 'fods', 'uos', 'sylk', 'dif', 'dbf', 'prn', 'qpw', '123', 'wb*', 'wq*', 'html', 'htm',
].map((x) => { return `.${x}` }).join(',')

function handleFileUpload(file) {
  /* Boilerplate to set up FileReader */
  const reader = new FileReader()
  reader.onload = (e) => {
    /* Parse data */
    const bstr = e.target.result
    const wb = read(bstr, { type: 'binary' })
    /* Get first worksheet */
    const wsname = wb.SheetNames[1]
    const ws = wb.Sheets[wsname]
    /* Convert array of arrays */
    data.value = utils.sheet_to_json(ws, { header: 1 })
  }

  reader.readAsBinaryString(file)
}

function handleFileChange(evt) {
  const files = evt.target.files
  if (files && files[0]) handleFileUpload(files[0])
}

const byLot = groupBy((row) => {
  return row['3']
})

const byTree = groupBy(row => row['5'])

const groupBylot = computed(() => {
  return byLot(data.value)
})

const report = computed(() => {
  delete groupBylot.value.Lote
  delete groupBylot.value[undefined]

  return Object.entries(groupBylot.value).reduce((memo, [lot, lotRows]) => {
    const fruitCount = lotRows.length
    const treeCount = Object.keys((lotRows.filter(row => row['6']))).length
    const whiteACount = +(countBy(row => row['14'])(lotRows).Sí || 0).toFixed(2)
    const toastACount = +(countBy(row => row['15'])(lotRows).Sí || 0).toFixed(2)
    const redACount = +(countBy(row => row['16'])(lotRows).Sí || 0).toFixed(2)
    const littleSpiderCount = +(countBy(row => row['17'])(lotRows).Sí || 0).toFixed(2)
    const healthyFruitCount = countBy(row => row['18'])(lotRows)

    memo[lot] = {
      treeCount,
      fruitCount,
      healthyFruitCount: +(healthyFruitCount.Sí).toFixed(2),
      healthyFruitAverage: +(healthyFruitCount.Sí / fruitCount * 100).toFixed(2),
      littleSpiderCount,
      littleSpiderAverage: +(littleSpiderCount / fruitCount * 100).toFixed(2),
      whiteACount,
      whiteAAverage: +(whiteACount / fruitCount * 100).toFixed(2),
      redACount,
      redAAverage: +(redACount / fruitCount * 100).toFixed(2),
      toastACount,
      toastAAverage: +(toastACount / fruitCount * 100).toFixed(2),
      magnifyingAAverage: +(sum(lotRows.filter(row => row['19']).map(row => row['19'])) / healthyFruitCount.No).toFixed(2),
    }

    return memo
  }, {})
})
</script>

<template>
  <input id="file" type="file" class="form-control" :accept="SheetJSFT" @change="handleFileChange" />
  <div v-if="Object.keys(report).length" class="max-w-8xl mx-auto sm:px-6 lg:px-4">
    <!-- Content goes here -->
    <ReportTable v-if="Object.keys(report).length" :report="report" class="mt-5" />
    <template v-for="(lot, lotName) in report" :key="lotName">
      <LotItem :lot="lot" :lot-name="lotName" />
    </template>
  </div>
</template>
