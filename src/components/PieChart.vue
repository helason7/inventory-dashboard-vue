<template>
  <Pie :data="chartData" :options="options" />
</template>

<script>
import { Pie } from 'vue-chartjs'
import {
  Chart as ChartJS,
  ArcElement, Legend, Tooltip
} from 'chart.js'

ChartJS.register(ArcElement, Legend, Tooltip)

export default {
  name: 'PieChart',
  components: { Pie },
  props: ['data'],
  computed: {
    chartData() {
      return {
        labels: this.data.map(item => item.name),
        datasets: [
          {
            backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444'],
            data: this.data.map(item => item.value),
          }
        ]
      }
    },
    options() {
      return {
        responsive: true,
        plugins: {
          legend: { position: 'bottom' }
        }
      }
    }
  }
}
</script>
