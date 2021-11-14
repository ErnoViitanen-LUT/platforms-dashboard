<template>
  <div id="app">
    <b-container>
      <b-row cols="1">
        <b-col>
          <b-card>
            <b-card-text>
              <h2 class="card-title">Surgical Tech Dashboard</h2>
            </b-card-text></b-card
          >
        </b-col>
      </b-row>
      <b-row cols-md="2" cols-lg="3">
        <b-col>
          <b-card title="Open alerts">
            <h2 class="card-title">4 pcs</h2>
          </b-card>
          <b-card title="line chart"><LineChart :chartData="barData" :options="lineOptions" /></b-card>
          <b-card><BarChart :chartData="mixedData" :options="barOptions" /></b-card>
        </b-col>
        <b-col>
          <b-card title="Card Title">
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>
          </b-card>
          <b-card><BarChart :chartData="barData" :options="horizontalOptions" /></b-card>
          <b-card><DoughnutChart :chartData="doughtnutData" :options="pieOptions" /></b-card>
          <b-card title="Card Title">
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>
          </b-card>
        </b-col>
        <b-col>
          <b-card title="Resolved incidents">
            <h2 class="card-title">349 pcs</h2>
          </b-card>
          <b-card><PieChart :chartData="pieData" :options="pieOptions" /></b-card>
          <b-card title="Card Title">
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>
          </b-card>
          <b-card><LineChart :chartData="mixedData" :options="lineOptions" /></b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import LineChart from './components/LineChart.vue'
import BarChart from './components/BarChart.vue'
import PieChart from './components/PieChart.vue'
import DoughnutChart from './components/DoughnutChart.vue'
import BarData from './assets/bar.json'
import PieData from './assets/pie.json'
import MixedData from './assets/mixed.json'
import DoughnutData from './assets/doughnut.json'

export default {
  name: 'App',
  components: {
    LineChart,
    BarChart,
    DoughnutChart,
    PieChart
  },
  data() {
    return {
      barData: BarData,
      pieData: PieData,
      mixedData: MixedData,
      doughtnutData: DoughnutData,

      lineOptions: {
        title: {
          display: true,
          text: 'Line Chart'
        },
        scales: {
          yAxes: [
            {
              ticks: {
                //min: 0,
                //max: 300,
                //stepSize: 100,
                //reverse: true
                //beginAtZero: true
              }
            }
          ]
        }
      },

      horizontalOptions: {
        title: {
          display: true,
          text: 'Another Bar Chart'
        },
        indexAxis: 'y',
        legend: {
          display: false
        },
        responsive: true,
        maintainAspectRatio: true
      },

      barOptions: {
        title: {
          display: true,
          text: 'Bar Chart'
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: true
              }
            }
          ],
          xAxes: [
            {
              gridLines: {
                display: false
              }
            }
          ]
        },
        legend: {
          display: true
        },
        responsive: true,
        maintainAspectRatio: false
      },

      pieOptions: {
        title: {
          display: true,
          text: 'Pie Chart'
        }
      }
    }
  },
  computed: {
    mobile() {
      return this.$vuetify.breakpoint.sm
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    fillData() {}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  color1: #196abb;
  margin-top: 20px;
}
.card {
  width: 100%;
  margin-bottom: 20px;
}
</style>
