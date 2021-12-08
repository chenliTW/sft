<script>
import { Line } from 'vue-chartjs';

export default {
   extends: Line,
   data() {
      return {
        datacollection: {
            labels: ['11:11','22:22','33:33','44:44','55:55','66:66','77:77','88:88','99:99'],
            datasets: []
            },
            options: {
            responsive: true,
            maintainAspectRatio: false,
            title: {
                display: true,
                text: ''
            },
            scales: {
                yAxes: [{
                    display: true,
                    ticks: {
                        stepSize: 1,
                        suggestedMin: 0,
                        //suggestedMax: 5.2,
                    },
                    unitStepSize: 1
                }]
            }
        },
        dataset:[
                {
                    label: 'critical',
                    data: [5,21,6,8,7,5,8,5,12],
                    lineTension: 0,   
                    backgroundColor: '#d9534f',
                    borderColor: '#d9534f',
                    borderWidth:10,
                    fill: false
                }, {
                    label: 'moderate',
                    data: [15,6,20,20,4,4,6,14,18,17],
                    lineTension: 0,   
                    backgroundColor: '#f0ad4e',
                    borderColor: '#f0ad4e',
                    borderWidth:5,
                    fill: false
                },{
                    label: 'info',
                    data: [26,8,9,15,7,20,21,3,25],
                    lineTension: 0,   
                    backgroundColor: '#5bc0de',
                    borderColor: '#5bc0de',
                    borderWidth:1,
                    fill: false
                }]
      }
   },
   mounted() {
        this.datacollection.datasets=this.dataset;
        this.renderChart(this.datacollection, this.options);
      
   }
}
</script>