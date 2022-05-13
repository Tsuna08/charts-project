<template>
  <v-card>
    <vue-highcharts :options="chart.chartOptions" ref="chartLine"> </vue-highcharts>
    <v-expansion-panels>
      <ExpansionPanel v-for="line in lineItems" :key="line._colorIndex">
        <template slot="panel-header"> Line settings {{ line.name }}</template>
        <template slot="panel-content">
          <Radio
            label="Chart type"
            :items="radioValue"
            :value="line.radioModel"
            @input="changeType(line)"
            v-model="line.radioModel"
          />
          <ColorPicker
            label="Chart color"
            :value="line.backgroundColor"
            @input="changeBackground(line)"
            v-model="line.backgroundColor"
          />
        </template>
      </ExpansionPanel>
    </v-expansion-panels>
  </v-card>
</template>

<script>
import VueHighcharts from 'vue2-highcharts'
import ExpansionPanel from '../components/ExpansionPanel'
import Radio from '../components/fields/Radio'
import ColorPicker from '../components/fields/ColorPicker'

export default {
  components: {
    VueHighcharts,
    ExpansionPanel,
    Radio,
    ColorPicker
  },
  data() {
    return {
      radioValue: ['line', 'bar'],
      chart: {},
      lineItems: []
    }
  },
  created() {
    this.createChart()
    this.chart.chartOptions.series[0].backgroundColor = '#80cbc4'
    this.chart.chartOptions.series[1].backgroundColor = '#263238'
    this.chart.chartOptions.series[2].backgroundColor = '#a5d6a7'

    this.lineItems = this.chart.chartOptions.series
    this.lineItems.forEach((element) => {
      element.radioModel = 'line'
    })
  },
  methods: {
    changeType(line) {
      const lineCharts = this.$refs
      lineCharts.chartLine.chart.series[line._colorIndex].update(
        {
          type: line.radioModel === 'bar' ? 'column' : 'spline'
        },
        true
      )
    },
    changeBackground(line) {
      const lineCharts = this.$refs
      lineCharts.chartLine.chart.series[line._colorIndex].update(
        {
          color: line.backgroundColor
        },
        true
      )
    },
    createChart() {
      this.chart = {
        chartOptions: {
          chart: {
            type: 'spline'
          },
          title: {
            text: 'Monthly Average Temperature'
          },
          xAxis: {
            categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
          },
          yAxis: {
            title: {
              text: 'Temperature'
            }
          },
          legend: {
            align: 'left',
            verticalAlign: 'top',
            borderWidth: 0
          },
          plotOptions: {
            series: {
              label: {
                connectorAllowed: false
              },
              pointStart: 1
            }
          },
          series: [
            {
              name: 'Tokyo',
              marker: {
                symbol: 'square'
              },
              data: [
                ...this.getRandomArbitrary(3, 15),
                {
                  y: 26.5,
                  marker: {
                    symbol: 'url(https://www.highcharts.com/samples/graphics/sun.png)'
                  }
                }
              ]
            },
            {
              name: 'London',
              marker: {
                symbol: 'diamond'
              },
              data: [
                {
                  y: 3.9,
                  marker: {
                    symbol: 'url(https://www.highcharts.com/samples/graphics/snow.png)'
                  }
                },
                ...this.getRandomArbitrary(5, 20)
              ]
            },
            {
              name: 'Paris',
              data: [
                {
                  y: 12.5
                },
                ...this.getRandomArbitrary(8, 25)
              ]
            }
          ],
          responsive: {
            rules: [
              {
                condition: {
                  maxWidth: 25
                },
                chartOptions: {
                  legend: {
                    layout: 'horizontal',
                    align: 'center',
                    verticalAlign: 'bottom'
                  }
                }
              }
            ]
          }
        }
      }
    },
    getRandomArbitrary(min, max) {
      const randomData = []
      for (let i = 0; i < 12; i++) {
        randomData[i] = Math.random() * (max - min) + min
      }
      return randomData
    }
  }
}
</script>
<style lang="sass" scoped>
.colorInput
  height: 4.5em
  margin-left: -12px
.addRow
  padding-top: 10px
.addCard
  padding: 0 8px
.filterButton
  margin-bottom: 10px
</style>
