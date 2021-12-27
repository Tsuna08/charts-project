<template>
  <v-container>
    <div v-for="chart in charts" :key="chart.id">
      <v-card elevation="2">
        <line-charts :styles="myStyles" :chartdata="chart.chartdata" :options="chart.options" />
      </v-card>
    </div>
    <v-card elevation="2">
      <v-icon x-large @click="createChart()">mdi-plus</v-icon>
    </v-card>
    <button @click="increase()">Increase height</button>
  </v-container>
</template>

<script>
import LineCharts from '../components/LineCharts'

export default {
  name: 'Charts',
  components: {
    LineCharts
  },
  data() {
    return {
      height: 300,
      charts: [
        {
          id: 1,
          chartdata: {
            labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
            datasets: [
              {
                label: 'Data One',
                backgroundColor: '#f87979',
                data: [40, 39, 10, 40, 39, 80, 40]
              }
            ]
          },
          options: { responsive: true, maintainAspectRatio: false }
        }
      ]
    }
  },
  methods: {
    increase() {
      this.height += 10
    },
    createChart() {
      this.charts.push({
        id: 1,
        chartdata: {
          labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
          datasets: [
            {
              label: 'Data One1',
              backgroundColor: '#f87979',
              data: [12, 65, 10, 23, 67, 21, 76]
            }
          ]
        },
        options: { responsive: true, maintainAspectRatio: false }
      })
    }
  },
  computed: {
    myStyles() {
      return {
        height: `${this.height}px`,
        position: 'relative',
        backgroundColor: '#add8e6'
      }
    }
  }
}
</script>
