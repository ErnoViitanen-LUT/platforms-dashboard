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
      <b-row cols="2">
        <b-col>
          <card :title="`Open alerts`" :text-large="openAlerts"></card>
        </b-col>
        <b-col>
          <card :title="`Resolved incidents`" :text-large="resolvedIncidents"></card>
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
          <card v-if="rooms[0]" :title="`Status of Room 1`" :text="status1" :items="rooms[0].statuslist"></card>
          <b-card title="line chart"><LineChart :chartData="dataBar" :options="lineOptions" /></b-card>
          <b-card><BarChart :chartData="dataMixed" :options="barOptions" /></b-card>
        </b-col>
        <b-col>
          <card v-if="rooms[1]" :title="`Status of Room 2`" :text="status2" :items="rooms[1].statuslist"></card>
          <b-card><BarChart :chartData="dataBar" :options="horizontalOptions" /></b-card>
          <b-card><DoughnutChart :chartData="dataDoughtnut" :options="pieOptions" /></b-card>

          <card
            :title="`Card Title`"
            :text="`Some quick example text to build on the card title and make up the bulk of the card's content.`"
          >
          </card>
        </b-col>
        <b-col>
          <card v-if="rooms[2]" :title="`Status of Room 3`" :text="status3" :items="rooms[2].statuslist"></card>
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
import FileRoom1 from './assets/room1.json'
import FileRoom2 from './assets/room2.json'
import FileRoom3 from './assets/room3.json'

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
      rooms: [],
      resolvedIncidentsCount: 0,
      isSimulating: false,
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
    },
    status1() {
      return this.getStatusText(this.rooms[0])
    },
    status2() {
      return this.getStatusText(this.rooms[1])
    },
    status3() {
      return this.getStatusText(this.rooms[2])
    },
    openAlerts() {
      let total = 0
      this.rooms.forEach((room) => {
        total += room.statuslist.filter((status) => status.status !== 'success').length
      })
      return total + ' pcs'
    },
    resolvedIncidents() {
      return this.resolvedIncidentsCount + ' pcs'
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    getStatusText(room) {
      let statustext = ''
      if (room) {
        const roomStatus = room.statuslist.find(({ status }) => status !== 'success')
        if (roomStatus == undefined) statustext = 'Ready to use'
        else {
          statustext = 'Unavailable'
        }
      }
      return statustext
    },
    simulateRandom() {
      let i = Math.floor(Math.random() * 3)
      let j = Math.floor(Math.random() * 3)
      let k = Math.floor(Math.random() * 3)

      this.rooms[i].statuslist[j].status = ['danger', 'success', 'warning'][k]
      if (this.rooms[i].statuslist[j].status === 'success') this.resolvedIncidentsCount++
      if (this.isSimulating) setTimeout(this.simulateRandom, 300)
    },
    fillData() {
      this.rooms = []
      this.rooms.push(FileRoom1)
      this.rooms.push(FileRoom2)
      this.rooms.push(FileRoom3)
    },
    simulateError() {
      this.isSimulating = false

      let i = Math.floor(Math.random() * 3)
      let j = Math.floor(Math.random() * 3)

      const roomStatus = this.rooms[i].statuslist.find(({ status }) => status !== 'danger')
      if (roomStatus) roomStatus.status = 'danger'
    },
    simulateSuccess() {
      this.isSimulating = false
      this.rooms.forEach((room) => {
        room.statuslist
          .filter((status) => status.status !== 'success')
          .forEach((status) => {
            status.status = 'success'
            this.resolvedIncidentsCount++
          })
      })
    },
    simulateIncident() {
      this.isSimulating = !this.isSimulating
      this.simulateRandom()
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
