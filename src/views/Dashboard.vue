
<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">Dashboard Manajemen Inventory</h1>

    <div class="mb-4">
      <label class="mr-2">Tanggal Mulai:</label>
      <input type="date" v-model="filters.start" class="border p-1 rounded" />
      <label class="ml-4 mr-2">Tanggal Akhir:</label>
      <input type="date" v-model="filters.end" class="border p-1 rounded" />
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div class="border rounded p-4 bg-white shadow">
        <h2 class="font-semibold mb-2">Penjualan per Bulan</h2>
        <BarChart :data="salesPerMonth" />
      </div>

      <div class="border rounded p-4 bg-white shadow">
        <h2 class="font-semibold mb-2">Penjualan per Grup Barang</h2>
        <PieChart :data="salesPerGroup" />
      </div>

      <div class="md:col-span-2 border rounded p-4 bg-white shadow">
        <h2 class="font-semibold mb-2">10 Produk Terlaris</h2>
        <BarChart :data="topProducts" />
      </div>
    </div>
  </div>
</template>

<script>
import BarChart from '@/components/BarChart.vue'
import PieChart from '@/components/PieChart.vue' 
import axios from 'axios'

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Dashboard',
  components: { BarChart, PieChart },
  data() {
    return {
      filters: {
        start: '2025-01-01',
        end: '2025-12-31'
      },
      salesPerMonth: [],
      salesPerGroup: [],
      topProducts: []
    }
  },
  watch: {
    filters: {
      deep: true,
      handler() {
        this.fetchData()
      }
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    async fetchData() {

      const params = {
        start: this.filters.start,
        end: this.filters.end
      }
    // Sales Per Month
    try {
      const response1 = await axios.get('http://localhost:3000/dashboard/sales-per-month', {
        params
      })
      const response2 = await axios.get('http://localhost:3000/dashboard/sales-per-group', {
        params
      })
      const response3 = await axios.get('http://localhost:3000/dashboard/top-products', {
        params
      })
      console.log(response1)
      this.salesPerMonth = response1.data
      this.salesPerGroup = response2.data
      this.topProducts = response3.data

    } catch (error) {
      console.error('Gagal mengambil data:', error)
    }

    }
  }
}
</script>

<style scoped>
input {
  max-width: 150px;
}
</style>
