<script>
import { PolarArea, Pie, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins

export default {
  extends: PolarArea,
  mixins: [reactiveProp],
  props: {
    chartData: {
      type: Object,
      default: null
    },
    options: {
      type: Object,
      default: {
        title: {
          display: true,
          text: 'Title'
        }
      }
    }
  },
  mounted() {
    this.renderChart(this.chartData, this.options)
  }
}
</script>
