<template>
  <div>
    <v-btn class="filterButton" color="text--darken-4" @click="sortByDate()">
      {{ this.ascending ? 'Ascending' : 'Dscending' }} by date create
    </v-btn>
    <v-row dense>
      <v-col v-for="chart in charts" :key="chart.id" :cols="6">
        <v-card>
          <v-card-actions class="justify-end">
            <v-icon v-show="showIconCross" small @click="deleteChart(chart.id)">mdi-close</v-icon>
          </v-card-actions>
          <vue-highcharts :options="chart.chartOptions" :ref="`${'chartLine_' + chart.id}`"></vue-highcharts>

          <v-card-actions class="justify-space-between">
            <div>
              <label>Chart type</label>
              <v-radio-group v-model="chart.radioModel" row>
                <v-radio
                  @click="changeType(chart.id)"
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
                @input="changeBackground(chart.id)"
                v-model="chart.backgroundColor"
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
        </v-card>
      </v-col>
    </v-row>

    <v-row dense class="justify-center addButton">
      <v-btn color="text--darken-4" :disabled="showButtonAdd" @click="createChart()">
        Add chart<v-icon middle>mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </div>
</template>

<script>
import VueHighcharts from 'vue2-highcharts'

export default {
  components: {
    VueHighcharts
  },
  data() {
    return {
      showIconCross: false,
      showButtonAdd: false,
      ascending: true,
      radioValue: ['line', 'bar'],

      charts: [
        {
          id: 1,
          createDate: new Date(),
          radioModel: 'line',
          backgroundColor: '#000000',
          chartOptions: {
            title: {
              text: 'Solar Employment Growth by Sector, 2010-2016'
            },
            yAxis: {
              title: {
                text: 'Number of Employees'
              }
            },
            xAxis: {
              title: {
                text: 'Number of Employees'
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
                pointStart: 2010
              }
            },
            series: [
              {
                name: 'Installation',
                data: [43.934, 52.503, 57.177, 69.658, 97.031, 119.931, 137.133, 154.175]
              }
              // {
              // name: 'New York',
              // data: [83.6, 78.8, 98.5, 93.4, 106.0, 84.5, 105.0, 104.3, 91.2, 83.5, 106.6, 92.3]
              // }
            ],
            responsive: {
              rules: [
                {
                  condition: {
                    maxWidth: 500
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
      ]
    }
  },
  methods: {
    sortByDate() {
      this.charts.sort((a, b) => {
        const fistTime = new Date(b.createDate)
        const lastTime = new Date(a.createDate)
        return this.ascending ? fistTime - lastTime : lastTime - fistTime
      })
      this.ascending = !this.ascending
    },
    changeType(id) {
      const chart = this.charts
        .filter((item) => {
          if (item.id === id) return item
        })
        .pop()
      const lineCharts = this.$refs
      lineCharts['chartLine_' + id][0].chart.series[0].update(
        {
          type: chart.radioModel === 'bar' ? 'column' : chart.radioModel
        },
        true
      )
    },
    changeBackground(id) {
      const chart = this.charts
        .filter((item) => {
          if (item.id === id) return item
        })
        .pop()
      const lineCharts = this.$refs
      lineCharts['chartLine_' + id][0].chart.series[0].update(
        {
          color: chart.backgroundColor
        },
        true
      )
    },
    deleteChart(id) {
      this.charts = this.charts.filter((item) => {
        if (item.id !== id) return item
      })

      if (this.charts.length === 1) {
        this.showIconCross = false
      }
      this.showButtonAdd = false
    },
    createChart() {
      if (this.charts.length < 4) {
        this.charts.push({
          id: this.charts.length + 1,
          createDate: new Date(),
          radioModel: 'line',
          backgroundColor: '#000000',
          chartOptions: {
            title: {
              text: 'Solar Employment Growth by Sector, 2010-2016'
            },
            yAxis: {
              title: {
                text: 'Number of Employees'
              }
            },
            xAxis: {
              title: {
                text: 'Number of Employees'
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
                pointStart: 2010
              }
            },
            series: [
              {
                name: 'Installation',
                data: [43.934, 52.503, 57.177, 69.658, 97.031, 119.931, 137.133, 154.175]
              },
              {
                name: 'New York',
                data: [83.6, 78.8, 98.5, 93.4, 106.0, 84.5, 105.0, 104.3, 91.2, 83.5, 106.6, 92.3]
              }
            ],
            responsive: {
              rules: [
                {
                  condition: {
                    maxWidth: 500
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
        })
        this.showIconCross = true
        if (this.charts.length === 4) {
          this.showButtonAdd = true
        }
      }
    }
  }
}
</script>
<style lang="sass" scoped>
.colorInput
  height: 4.5em
  margin-left: -12px
.addButton
  padding-top: 10px
.filterButton
  margin-bottom: 10px
</style>
