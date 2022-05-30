<template>
  <highcharts :options="chartOptions" />
</template>

<script>
import { Chart } from 'highcharts-vue'

export default {
  components: {
    highcharts: Chart
  },
  props: ['chart', 'typeChart', 'colorChart', 'labelChart'],
  computed: {
    chartOptions() {
      return {
        chart: {
          type: this.typeChart,
          margin: [70, 20, 32, 70]
        },
        title: { text: this.labelChart },
        xAxis: {
          // categories: setTime(60, 'hour'),
          title: null
        },
        yAxis: {
          // categories: setCategories(101, 'С°'),
          title: { text: this.labelChart }
        },
        series: [
          {
            name: this.typeChart,
            data: this.chart,
            color: this.colorChart
          }
        ],
        legend: {
          enabled: false
        },
        tooltip: {
          enabled: true,
          crosshairs: false
        },
        plotOptions: {
          series: {
            showInLegend: true
          }
        }
      }
    }
    // }
  }
}
</script>
