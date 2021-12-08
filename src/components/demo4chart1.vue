<script>
import { Bar } from 'vue-chartjs';

export default {
   extends: Bar,
   data() {
      return {
        datacollection: {
            labels: ['1','2','3'],
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
                        suggestedMax: 100,
                    },
                    unitStepSize: 1
                }]
            }
        },
        dataset:[
                {
                    label: '答對率',
                    data: [100,78,37],
                    lineTension: 0,   
                    backgroundColor: ['green','green','green','green'],
                    borderColor: ['green','green','green','green'],
                    borderWidth:10,
                    fill: false
                }]
      }
   },
   mounted() {
        this.datacollection.datasets=this.dataset;
   },
   methods:{

   },
   props:['ren'],
   watch:{
       ren: function(){
           setTimeout(()=>{this.renderChart(this.datacollection, this.options)},100);
       }
   }
}
</script>