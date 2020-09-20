<template>
  <div class="flex flex-col h-screen">
    <h1 class="bg-green-500 text-white text-center text-4xl">COVID-19 Stats</h1>
    <select v-model="stateSelected">
      <option v-for="st in states" :key="st" :value="st[1]">{{ st[0] }}</option>
    </select>
    <div class="p-3 bg-gray-200 flex-1">
      <canvas class="bg-white" id="positive-test-ratio"></canvas>
    </div>
  </div>
</template>

<script>
import 'chart.js'
import axios from 'axios'
import { DateTime } from 'luxon'

export default {
  data() {
    return {
      positiveTestRatioChart: null,
      stateSelected: 'MT',
      states: [
        ['Arizona', 'AZ'],
        ['Alabama', 'AL'],
        ['Alaska', 'AK'],
        ['Arkansas', 'AR'],
        ['California', 'CA'],
        ['Colorado', 'CO'],
        ['Connecticut', 'CT'],
        ['Delaware', 'DE'],
        ['Florida', 'FL'],
        ['Georgia', 'GA'],
        ['Hawaii', 'HI'],
        ['Idaho', 'ID'],
        ['Illinois', 'IL'],
        ['Indiana', 'IN'],
        ['Iowa', 'IA'],
        ['Kansas', 'KS'],
        ['Kentucky', 'KY'],
        ['Louisiana', 'LA'],
        ['Maine', 'ME'],
        ['Maryland', 'MD'],
        ['Massachusetts', 'MA'],
        ['Michigan', 'MI'],
        ['Minnesota', 'MN'],
        ['Mississippi', 'MS'],
        ['Missouri', 'MO'],
        ['Montana', 'MT'],
        ['Nebraska', 'NE'],
        ['Nevada', 'NV'],
        ['New Hampshire', 'NH'],
        ['New Jersey', 'NJ'],
        ['New Mexico', 'NM'],
        ['New York', 'NY'],
        ['North Carolina', 'NC'],
        ['North Dakota', 'ND'],
        ['Ohio', 'OH'],
        ['Oklahoma', 'OK'],
        ['Oregon', 'OR'],
        ['Pennsylvania', 'PA'],
        ['Rhode Island', 'RI'],
        ['South Carolina', 'SC'],
        ['South Dakota', 'SD'],
        ['Tennessee', 'TN'],
        ['Texas', 'TX'],
        ['Utah', 'UT'],
        ['Vermont', 'VT'],
        ['Virginia', 'VA'],
        ['Washington', 'WA'],
        ['West Virginia', 'WV'],
        ['Wisconsin', 'WI'],
        ['Wyoming', 'WY'],
      ]
    }
  },
  
  methods: {
    getStateData(state) {
      var self = this

      axios.get(`https://api.covidtracking.com/v1/states/${state.toLowerCase()}/daily.json`)
      .then((result) => {
        let ratios = result.data.slice(0, 30).reverse().map(r => {
          let newDate = DateTime.fromISO(r.date).toISODate()

          return {
            x: newDate,
            y: r.positiveIncrease / r.totalTestResultsIncrease
          }
        })
        console.log(ratios)

        for (let r of ratios) {
          self.positiveTestRatioChart.data.labels.pop()
          self.positiveTestRatioChart.data.datasets.forEach((dataset) => {
            dataset.data.pop()
          })
        }

        for (let r of ratios) {
          self.positiveTestRatioChart.data.labels.push(r.x)
          self.positiveTestRatioChart.data.datasets.forEach((dataset) => {
            dataset.data.push(r)
          })
        }

        self.positiveTestRatioChart.update()
      })
    }
  },

  mounted() {
    this.positiveTestRatioChart = new Chart(document.getElementById('positive-test-ratio'), {
      type: 'line',
      data: {
        labels: [],
        datasets: [{
            label: 'Ratio',
            data: []
        }]
      },
    })

    this.getStateData(this.stateSelected)
  },

  watch: {
    stateSelected(newState) {
      this.getStateData(newState)
    }
  }
}
</script>
