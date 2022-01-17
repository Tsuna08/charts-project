<template>
  <v-card>
    <vue-highcharts :options="chart.chartOptions" ref="chartLine"></vue-highcharts>

    <v-expansion-panels>
      <v-expansion-panel v-for="line in lineItems" :key="line._colorIndex">
        <v-expansion-panel-header> Line settings {{ line.name }} </v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-card-actions class="justify-space-between">
            <div>
              <label>Chart type</label>
              <v-radio-group v-model="line.radioModel" row>
                <v-radio
                  @click="changeType(line)"
                  v-for="radio in radioValue"
                  :key="radio"
                  :label="radio"
                  :value="radio"
                ></v-radio>
              </v-radio-group>
            </div>

            <div>
              <label>Chart color</label>
              <v-color-picker
                class="colorInput"
                @input="changeBackground(line)"
                v-model="line.backgroundColor"
                dot-size="25"
                hide-canvas
                hide-inputs
                hide-sliders
                mode="hsla"
                show-swatches
                swatches-max-height="200"
              ></v-color-picker>
            </div>
          </v-card-actions>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-card>
</template>

<script>
import VueHighcharts from 'vue2-highcharts'

export default {
  components: {
    VueHighcharts
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
