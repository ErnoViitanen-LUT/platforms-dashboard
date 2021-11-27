<template>
  <div id="app">
    <b-container>
      <b-row cols="1">
        <b-col>
          <b-card>
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
      <b-row lg="1">
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
          <card :title="`Completed tasks`" :text-large="completedTasks"></card>
        </b-col>
      </b-row>

      <b-row>
        <b-col md="6" lg="4">
          <card v-if="rooms[0]" :title="`Status of Room 1`" :text="status1" :items="rooms[0].statuslist"></card>
        </b-col>
        <b-col>
          <card v-if="rooms[1]" :title="`Status of Room 2`" :text="status2" :items="rooms[1].statuslist"></card>
        </b-col>
        <b-col>
          <card v-if="rooms[2]" :title="`Status of Room 3`" :text="status3" :items="rooms[2].statuslist"></card>
        </b-col>
      </b-row>
      <b-row cols-md="1" cols-lg="2">
        <b-col md="12" lg="4">
          <b-card title="Active Devices (#)"
            ><doughnutChart :chartData="activeDevicesData" :options="activeDevicesOptions"
          /></b-card>
        </b-col>
        <b-col md="12" lg="8">
          <b-card title="Latency (ms)"><LineChart :chartData="latencyData" :options="latencyOptions" /></b-card>
        </b-col>
      </b-row>
      <b-row cols-md="1" cols-lg="2">
        <b-col md="12" lg="4">
          <b-card title="Power Consumption (kWh)"
            ><BarChart :chartData="powerConsumptionData" :options="powerConsumptionOptions"
          /></b-card>
        </b-col>
        <b-col md="12" lg="8">
          <b-card title="Maintenance Hours"
            ><BarChart :chartData="maintenanceData" :options="maintenanceOptions"
          /></b-card>
        </b-col>
        <b-col> </b-col>
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
      colorNone: 'rgba(255, 99, 132, 0.0)',
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(255, 119, 74, 0.2)',
        'rgba(255, 205, 86, 0.2)',
        'rgba(75, 192, 192, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(153, 102, 255, 0.2)',
        'rgba(201, 203, 207, 0.2)'
      ],
      borderColor: [
        'rgb(255, 99, 132)',
        'rgb(255, 159, 64)',
        'rgb(255, 205, 86)',
        'rgb(75, 192, 192)',
        'rgb(54, 162, 235)',
        'rgb(153, 102, 255)',
        'rgb(201, 203, 207)'
      ],
      simuStatus: 'success',
      rooms: [],
      completedTasksCount: 0,
      isSimulating: false,
      latencyOptions: {
        title: {
          display: false
        },
        scales: {
          yAxes: [
            {
              ticks: {
                //min: 0,
                //max: 300,
                stepSize: 5
                //reverse: true
                //beginAtZero: true
              }
            }
          ]
        },
        responsive: true,
        maintainAspectRatio: false
      },
      powerConsumptionOptions: {
        title: {
          display: false
        },
        scales: {
          yAxes: [
            {
              stacked: true,
              ticks: {
                beginAtZero: true,
                stepSize: 0.5
              },
              gridLines: {
                display: true
              }
            }
          ],
          xAxes: [
            {
              stacked: true,
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
      maintenanceOptions: {
        title: {
          display: false
        },
        legend: {
          display: true
        },
        responsive: true,
        maintainAspectRatio: false
      },
      activeDevicesOptions: {
        title: {
          display: false
        },
        legend: {
          display: true
        },
        responsive: true,
        maintainAspectRatio: false
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
    completedTasks() {
      return this.completedTasksCount + ' pcs'
    },
    activeDevicesData() {
      if (this.rooms.length === 3) {
        let data = {
          labels: [this.rooms[0].name, this.rooms[1].name, this.rooms[2].name],
          datasets: [
            {
              data: [this.rooms[0].activeDevices, this.rooms[1].activeDevices, this.rooms[2].activeDevices],
              backgroundColor: [this.backgroundColor[5], this.backgroundColor[1], this.backgroundColor[4]],
              borderColor: [this.borderColor[5], this.borderColor[1], this.borderColor[4]],
              hoverOffset: 4,
              borderWidth: 1
            }
          ]
        }
        return data
      }
      return {}
    },
    latencyData() {
      if (this.rooms.length === 3) {
        let d = new Date()
        let h = d.getHours()
        let m = d.getMinutes()
        let labels = []
        for (let i = 0; i < this.rooms[0].latency.length; i++) {
          d.setMinutes(m - 10)
          m = d.getMinutes()
          labels.push(h.toString() + ':' + (m < 10 ? '0' : '') + m.toString())
        }
        let data = {
          labels: labels.reverse(),
          datasets: [
            {
              label: this.rooms[0].name,
              data: this.rooms[0].latency,
              backgroundColor: this.colorNone,
              borderColor: this.borderColor[5],
              hoverOffset: 4,
              borderWidth: 1
            },
            {
              label: this.rooms[1].name,
              data: this.rooms[1].latency,
              backgroundColor: this.colorNone,
              borderColor: this.borderColor[2],
              hoverOffset: 4,
              borderWidth: 1
            },
            {
              label: this.rooms[2].name,
              data: this.rooms[2].latency,
              backgroundColor: this.colorNone,
              borderColor: this.borderColor[4],
              hoverOffset: 4,
              borderWidth: 1
            }
          ]
        }
        return data
      }
      return {}
    },
    maintenanceData() {
      if (this.rooms.length === 3) {
        let deviceDataset = []
        let labels = []
        this.rooms.forEach((room, i) => {
          labels.push(room.name)

          room.devices.forEach((device, i) => {
            let dev = deviceDataset.find((d) => d.label === device.name)
            if (dev) {
              dev.data.push(device.maintenanceHours)
            } else {
              deviceDataset.push({
                type: 'bar',
                label: device.name,
                backgroundColor: this.backgroundColor[i],
                borderColor: this.borderColor[i],
                data: [device.maintenanceHours],
                borderWidth: 1
              })
            }
          })
        })
        let data = {
          labels: labels,
          datasets: deviceDataset
        }

        return data
      }
      return {}
    },
    powerConsumptionData() {
      if (this.rooms.length === 3) {
        let d = new Date()
        let h = d.getHours()
        let m = d.getMinutes()
        let labels = []
        for (let i = 0; i < this.rooms[0].powerConsumption.length; i++) {
          d.setHours(h - 1)
          h = d.getHours()
          labels.push(h.toString() + ':' + (m < 10 ? '0' : '') + m.toString())
        }
        let data = {
          labels: labels.reverse(),
          scales: {
            x: {
              stacked: true
            },
            y: {
              stacked: true
            }
          },
          datasets: [
            {
              backgroundColor: this.backgroundColor[5],
              borderColor: this.borderColor[5],
              borderWidth: 1,
              label: this.rooms[0].name,
              data: this.rooms[0].powerConsumption
            },
            {
              backgroundColor: this.backgroundColor[1],
              borderColor: this.borderColor[1],
              borderWidth: 1,
              label: this.rooms[1].name,
              data: this.rooms[1].powerConsumption
            },
            {
              backgroundColor: this.backgroundColor[4],
              borderColor: this.borderColor[4],
              borderWidth: 1,
              label: this.rooms[2].name,
              data: this.rooms[2].powerConsumption
            }
          ]
        }
        return data
      }
      return {}
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
      if (this.rooms[i].statuslist[j].status === 'success') this.completedTasksCount++

      this.rooms[j].activeDevices += Math.max(Math.round(Math.random()) * 3 - 1, 0)
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
        room.activeDevices = 2
        room.statuslist
          .filter((status) => status.status !== 'success')
          .forEach((status) => {
            status.status = 'success'
            this.completedTasksCount++
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
