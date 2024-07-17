<template>
    <div v-if="chartData">
        <line-chart :chart-data="chartData" :options="chartOptions"></line-chart>
    </div>
    <div v-else>
        Loading...
    </div>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart, registerables } from 'chart.js'
Chart.register(...registerables)

export default {
    name: 'MeanBestIndividualsChart',
    props: ['data'],
    components: {
        LineChart: Line
    },
    computed: {
        chartData() {
            if (!this.data || !Array.isArray(this.data)) return null
            return {
                labels: this.data.map((_, index) => `Generation ${index + 1}`),
                datasets: [
                    {
                        label: 'Mean Best Individuals',
                        backgroundColor: '#36A2EB',
                        data: this.data.map(generation => generation[1])
                    }
                ]
            }
        },
        chartOptions() {
            return {
                responsive: true,
                maintainAspectRatio: false
            }
        }
    }
}
</script>
