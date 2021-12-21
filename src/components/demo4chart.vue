<script>
import { Bar } from 'vue-chartjs';

export default {
   extends: Bar,
   data() {
      return {
        datacollection: {
            labels: ['A','B','C','D'],
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
                    label: '最多人選擇',
                    data: [2,1,1,1],
                    lineTension: 0,   
                    backgroundColor: ['black','gray','gray','gray'],
                    borderColor: ['black','gray','gray','gray'],
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