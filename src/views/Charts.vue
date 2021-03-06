<template>
  <div>
    <v-btn class="filterButton" outlined color="teal text--darken-4" @click="sortByDate()">
      {{ this.ascending ? 'Ascending' : 'Dscending' }} by date create
    </v-btn>
    <v-row dense>
      <v-col v-for="chart in charts" :key="chart.typeChart" min-width="700">
        <Card className="justify-end">
          <template slot="card-actions">
            <v-icon v-show="showIconCross" small @click="deleteChart(chart.typeChart)">mdi-close</v-icon>
          </template>
          <template slot="card-content">
            <ChartBlock
              :chart="chart.series"
              :typeChart="chart.radioModel"
              :colorChart="chart.backgroundColor"
              :labelChart="chart.typeChart"
              min-width="700"
            />
            <v-expansion-panels>
              <ExpansionPanel>
                <template slot="panel-header"> Chart settings </template>
                <template slot="panel-content">
                  <Radio label="Type" :items="radioValue" :value="chart.radioModel" v-model="chart.radioModel" />
                  <ColorPicker label="Color" :value="chart.backgroundColor" v-model="chart.backgroundColor" />
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
import ChartBlock from '../components/ChartBlock'
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
    ChartBlock,
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
          backgroundColor: '#2196f3',
          typeChart: typeChart.charAt(0).toUpperCase() + typeChart.slice(1),
          series: this.getRandomArbitrary(scale[typeChart].min, scale[typeChart].max)
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
