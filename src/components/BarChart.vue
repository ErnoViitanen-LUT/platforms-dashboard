<script>
import { Bar, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins

export default {
  extends: Bar,
  mixins: [reactiveProp],
  props: {
    // Chart data
    chartData: {
      type: Object,
      required: true
    },
    // Optional settings for chart rendering
    options: {
      type: Object,
      default: {
        title: {
          display: true,
          text: 'Bar Chart Title'
        }
      }
    },
    // Optional events for chart
    events: {
      type: Array,
      required: false
    }
  },
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.renderChart(this.chartData, this.options)
    //this.renderChart(this.chartData, { onClick: this.handleEvent })
    this.chart = this.$data._chart
  },
  methods: {
    // @vuese
    // Used for handling chart events
    // @arg Event emitted from chart
    handleEvent(event) {
      const eventIndex = this.chart.getElementAtEvent(event)[0]._index
    }
  }
}
</script>
