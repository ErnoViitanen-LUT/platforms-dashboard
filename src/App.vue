<template>
  <div id="app">
    <b-container>
      <b-row cols="1">
        <b-col>
          <b-card>
            <b-card-text> <h2 class="card-title">Surgical Tech Dashboard</h2> </b-card-text>
          </b-card>
        </b-col>
      </b-row>
      <b-row cols="1">
        <b-col>
          <b-card>
            <b-card-text> <h4 class="card-title">Simulation Functions</h4> </b-card-text>

            <b-row cols="3">
              <b-col>
                <button @click="simulateIncident" type="button" class="btn btn-warning">Simulate Incident</button>
              </b-col>
              <b-col>
                <button @click="simulateError" type="button" class="btn btn-danger">Simulate Error</button>
              </b-col>
              <b-col>
                <button @click="simulateSuccess" type="button" class="btn btn-success">Simulate Success</button>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
      <b-row cols-md="2" cols-lg="3">
        <b-col>
          <card :title="`Open alerts`" :text-large="`4 pcs`"></card>
          <card :title="`Status of Room 1`" :text="`Ready to use`" :items="roomStatus.room1"></card>
          <b-card title="line chart"><LineChart :chartData="dataBar" :options="lineOptions" /></b-card>
          <b-card><BarChart :chartData="dataMixed" :options="barOptions" /></b-card>
        </b-col>
        <b-col>
          <card :title="`Status of Room 2`" :text="`Unavailable`" :items="roomStatus.room2"></card>
          <b-card><BarChart :chartData="dataBar" :options="horizontalOptions" /></b-card>
          <b-card><DoughnutChart :chartData="dataDoughtnut" :options="pieOptions" /></b-card>

          <card
            :title="`Card Title`"
            :text="`Some quick example text to build on the card title and make up the bulk of the card's content.`"
          >
          </card>
        </b-col>
        <b-col>
          <card :title="`Status of Room 3`" :text="`Unavailable`" :items="roomStatus.room3"></card>
          <card :title="`Resolved incidents`" :text-large="`349 pcs`"></card>
          <b-card><PieChart :chartData="dataPie" :options="pieOptions" /></b-card>
          <card
            :title="`Card Title`"
            :text="`Some quick example text to build on the card title and make up the bulk of the card's content.`"
          >
          </card>
          <b-card><LineChart :chartData="dataMixed" :options="lineOptions" /></b-card>
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
import List from './components/List.vue'
import Card from './components/Card.vue'
import FileBar from './assets/bar.json'
import FilePie from './assets/pie.json'
import FileMixed from './assets/mixed.json'
import FileDoughnut from './assets/doughnut.json'
import FileStatusOk from './assets/status-ok.json'
import FileStatusFail from './assets/status-fail.json'
import FileStatusMixed from './assets/status-mixed.json'

export default {
  name: 'App',
  components: {
    LineChart,
    BarChart,
    DoughnutChart,
    PieChart,
    List,
    Card
  },
  data() {
    return {
      dataBar: FileBar,
      dataPie: FilePie,
      dataMixed: FileMixed,
      dataDoughtnut: FileDoughnut,
      simuStatus: 'success',
      roomStatus: {
        room1: [],
        room2: [],
        room3: []
      },
      dataStatusMixed: FileStatusMixed,
      /*[
        { text: 'one', type: 'danger' },
        { text: 'two', type: 'info' },
        { text: 'three', type: 'success' },
        { text: 'four', type: 'warning' }
      ]*/
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
    fillData() {
      if (this.simuStatus === 'error') {
        this.roomStatus.room2 = FileStatusFail
      } else if (this.simuStatus === 'incident') {
        this.roomStatus.room1 = FileStatusFail
        this.roomStatus.room3 = FileStatusMixed
      } else {
        this.roomStatus.room1 = FileStatusOk
        this.roomStatus.room2 = FileStatusOk
        this.roomStatus.room3 = FileStatusOk
      }
    },
    simulateError() {
      this.simuStatus = 'error'
      this.fillData()
    },
    simulateSuccess() {
      this.simuStatus = 'success'
      this.fillData()
    },
    simulateIncident() {
      this.simuStatus = 'incident'
      this.fillData()
    }
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
