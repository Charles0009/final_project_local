<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,

  props: ['dates','selected','values'],
  methods: {

    render: function() {
      let dataset = {
        label: this.selected,
        backgroundColor: 'lightblue',
        borderColor: 'rgba(0,0,0,0)',
        data: this.values,
        lineTension: 0
      }
         
      let options = {
        labels: this.dates,
        datasets: [dataset], 
        options: {
          
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [ {
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false
              }
        }
            

          this.renderChart(options, {responsive: true, maintainAspectRatio: false})
    }
  },
    watch: {
      'selected': {
        handler: function () {
          this.render();
        },
        deep: true
      },
      'dates': {
        handler: function () {
          this.render();
        },
        deep: true
      },
      'values': {
        handler: function () {
          this.render();
        },
        deep: true
      },
    },
    mounted: function() {
      this.render();
    }
}
</script>
