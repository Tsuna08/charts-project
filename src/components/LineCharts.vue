<script>
// import { Line } from 'vue-chartjs'
// import Chart from 'chart.js'
// import { generateChart } from 'vue-chartjs'
// import 'chartjs-plugin-style'

// const CustomLine = generateChart('custom-line', 'LineWithLine')
// Chart.defaults.LineWithLine = Chart.defaults.line

// export default {
//   extends: CustomLine,
//   mounted() {
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: {
    chartdata: {
      type: Object,
      default: null
    },
    options: {
      type: Object,
      default: null
    }
  },
  mounted() {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>
