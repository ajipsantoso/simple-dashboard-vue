<script>
import { Doughnut } from 'vue-chartjs'

export default {
  extends: Doughnut,
  props: ["chartdata", "options"],
  mounted() {
    this.renderChart(
      {
        labels: [
          "North America",
          "South America",
          "Australia",
        ],
        datasets: [
         {
           data: [30, 60, 15],
           backgroundColor: [
                '#00A5FF',
                '#FFB056',
                '#00FF72',
            ],
            borderWidth: 1
         }
        ],
      },
      {
        legend: {
          position: 'bottom',
          display: true,
          labels: {
              usePointStyle: true,
          },
        },
        responsive:true,
        maintainAspectRatio: false,
      }
    );  
  },
  computed: {
    chartData: function() {
      return this.data;
    }
  },
  methods: {
  },
  watch: {
    data: function() {
      this._chart.destroy();
      //this.renderChart(this.data, this.options);
      this.renderLineChart();
    }
  }
}
</script>
