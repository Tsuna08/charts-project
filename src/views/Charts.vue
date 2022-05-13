<template>
  <div>
    <v-btn class="filterButton" outlined color="teal text--darken-4" @click="sortByDate()">
      {{ this.ascending ? 'Ascending' : 'Dscending' }} by date create
    </v-btn>
    <v-row dense>
      <v-col v-for="chart in charts" :key="chart.typeChart" :cols="6">
        <Card className="justify-end">
          <template slot="card-actions">
            <v-icon v-show="showIconCross" small @click="deleteChart(chart.typeChart)">mdi-close</v-icon>
          </template>
          <template slot="card-content">
            <vue-highcharts :options="chart.chartOptions" :ref="`${'chartLine_' + chart.typeChart}`"> </vue-highcharts>
            <v-expansion-panels>
              <ExpansionPanel>
                <template slot="panel-header"> Chart settings </template>
                <template slot="panel-content">
                  <Radio
                    label="Chart type"
                    :items="radioValue"
                    :value="chart.radioModel"
                    @input="changeType(chart.typeChart)"
                    v-model="chart.radioModel"
                  />
                  <ColorPicker
                    label="Chart color"
                    :value="chart.backgroundColor"
                    @input="changeBackground(chart.typeChart)"
                    v-model="chart.backgroundColor"
                  />
                </template>
              </ExpansionPanel>
            </v-expansion-panels>
          </template>
        </Card>
      </v-col>
    </v-row>

    <v-row dense class="justify-center addRow">
      <Card className="justify-space-between">
        <template slot="card-actions">
          <!-- <Radio :items="chartView" :value="chartModel" v-model="chartModel" /> -->
          <v-radio-group v-model="chartModel" row>
            <v-radio
              v-for="radio in chartView"
              v-show="!lastTypeChart.includes(radio)"
              :key="radio"
              :label="radio"
              :value="radio"
            ></v-radio>
          </v-radio-group>
          <v-btn dark color="teal text--darken-4" :disabled="showButtonAdd" @click="createChart(chartModel)">
            Add chart<v-icon middle>mdi-plus</v-icon>
          </v-btn>
        </template>
      </Card>
    </v-row>
  </div>
</template>

<script>
import VueHighcharts from 'vue2-highcharts'
import ExpansionPanel from '../components/ExpansionPanel'
import Card from '../components/Card'
import Radio from '../components/fields/Radio'
import ColorPicker from '../components/fields/ColorPicker'

const scale = {
  temperature: { max: -30, min: -10 },
  humidity: { max: 100, min: 0 },
  'wind speed': { max: 8, min: 0 },
  pressure: { max: 800, min: 700 }
}

export default {
  components: {
    VueHighcharts,
    Card,
    ExpansionPanel,
    Radio,
    ColorPicker
  },
  data() {
    return {
      showIconCross: false,
      showButtonAdd: false,
      ascending: true,
      radioValue: ['line', 'bar'],
      chartModel: '',
      chartTypeModel: '',
      chartView: ['temperature', 'humidity', 'wind speed', 'pressure'],
      lastTypeChart: [],
      charts: []
    }
  },
  created() {
    this.createChart('temperature')
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
    changeType(typeChart) {
      console.log('typeChart: ', typeChart)
      const chart = this.charts
        .filter((item) => {
          if (item.typeChart === typeChart) return item
        })
        .pop()
      console.log('chart.radioModel: ', chart.radioModel)
      const lineCharts = this.$refs
      lineCharts['chartLine_' + typeChart][0].chart.series[0].update(
        {
          type: chart.radioModel === 'bar' ? 'column' : 'spline'
        },
        true
      )
    },
    changeBackground(typeChart) {
      console.log('typeChart: ', typeChart)
      const chart = this.charts
        .filter((item) => {
          if (item.typeChart === typeChart) return item
        })
        .pop()
      const lineCharts = this.$refs
      console.log(' chart.backgroundColor: ', chart.backgroundColor)
      lineCharts['chartLine_' + typeChart][0].chart.series[0].update(
        {
          color: chart.backgroundColor
        },
        true
      )
    },
    deleteChart(typeChart) {
      this.charts = this.charts.filter((item) => {
        if (item.typeChart !== typeChart) return item
      })

      this.lastTypeChart = this.lastTypeChart.filter((item) => {
        if (item !== typeChart) return item
      })

      if (this.charts.length === 1) {
        this.showIconCross = false
      }
      this.showButtonAdd = false
    },
    createChart(typeChart) {
      if (this.charts.length < 4 && typeChart) {
        this.charts.push({
          createDate: new Date(),
          radioModel: 'line',
          backgroundColor: '#000000',
          typeChart: typeChart,
          chartOptions: {
            chart: {
              type: 'spline'
            },
            title: {
              text: typeChart
            },
            yAxis: {
              title: {
                text: typeChart
              }
            },
            xAxis: {
              title: {
                text: 'Days of December'
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
                name: typeChart,
                data: this.getRandomArbitrary(scale[typeChart].min, scale[typeChart].max)
              }
            ],
            responsive: {
              rules: [
                {
                  condition: {
                    maxWidth: scale[typeChart].max
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
        this.lastTypeChart.push(typeChart)
        this.chartModel = undefined
        if (this.charts.length !== 1) {
          this.showIconCross = true
        }
        if (this.charts.length === 4) {
          this.showButtonAdd = true
        }
      }
    },
    getRandomArbitrary(min, max) {
      const randomData = []
      for (let i = 0; i < 30; i++) {
        randomData[i] = Math.random() * (max - min) + min
      }
      return randomData
    }
  }
}
</script>
<style lang="sass" scoped>
.addRow
  padding-top: 10px
.addCard
  padding: 0 8px
.filterButton
  margin-bottom: 10px
</style>
