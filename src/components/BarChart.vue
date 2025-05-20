<template>
  <Bar :data="chartData" :options="options" />
</template>

<script>
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title, Tooltip, Legend,
  BarElement, CategoryScale, LinearScale
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  props: ['data'],
  computed: {
    chartData() {
      return {
        labels: this.data.map(item => item.name),
        datasets: [
          {
            label: 'Penjualan',
            backgroundColor: '#3b82f6',
            data: this.data.map(item => item.value),
          }
        ]
      }
    },
    options() {
      return {
        responsive: true,
        plugins: {
          legend: { display: false }
        }
      }
    }
  }
}
</script>
