<template>
    <div>
        <line-chart :chart-data="chartData" :options="chartOptions"></line-chart>
    </div>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart, registerables } from 'chart.js'
Chart.register(...registerables)

export default {
    name: 'BestIndividualsChart',
    props: ['data'],
    components: {
        LineChart: Line
    },
    computed: {
        chartData() {
            if (!this.data || !Array.isArray(this.data)) return { labels: [], datasets: [] }
            return {
                labels: this.data.map((_, index) => `Generation ${index + 1}`),
                datasets: [
                    {
                        label: 'Best Individuals',
                        backgroundColor: '#f87979',
                        data: this.data.map(generation => Math.max(...generation.map(individual => individual[1])))
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
