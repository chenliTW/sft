<template>
  <div id="app">
    <b-navbar type="dark" variant="dark">
      <b-navbar-brand href="#">
        LWEW DEMO
      </b-navbar-brand>
      <b-navbar-nav>
        <!--<b-nav-item @click="chgdemo('DEMO 1')">DEMO 1</b-nav-item>
        <b-nav-item @click="chgdemo('DEMO 2')">DEMO 2</b-nav-item>
        <b-nav-item @click="chgdemo('DEMO 3')">DEMO 3(學生端)</b-nav-item>
        <b-nav-item @click="chgdemo('DEMO 4')">DEMO 4(老師端)</b-nav-item>-->
        <b-nav-item @click="chgdemo('DEMO 5')">DEMO 5(互動展示)</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
    <b-container>
    <br>
    <div v-if="DEMO=='DEMO 2'">
      <b-button class="sidebarbtn" v-b-toggle.sidebar>打<br>開<br>好<br>站<br>連<br>結</b-button>
      <b-sidebar id="sidebar" title="好站連結" shadow>
        <div class="px-3 py-2" style="text-align:left;">
          <details>
            <summary>國文</summary>
          &nbsp;&nbsp;&nbsp;<b-link href="https://rueylin0119.pixnet.net/blog/post/262360434-%E9%BB%83%E6%98%A5%E6%98%8E%EF%BC%9A%E6%88%B0%E5%A3%AB%EF%BC%8C%E4%B9%BE%E6%9D%AF">樵客老師的國文教學網站</b-link><br>
          &nbsp;&nbsp;&nbsp;<b-link href="http://blog.udn.com/supergogo1999/article">隨寫人的教學與生命情懷</b-link>
          </details>
          <details>
            <summary>開放式課程</summary>
            &nbsp;&nbsp;&nbsp;<b-link href="http://ocw.aca.ntu.edu.tw/ntu-ocw/">台大開放式課程</b-link><br>
            &nbsp;&nbsp;&nbsp;<b-link href="https://tfgcoocs.fg.tp.edu.tw/">北一女中開放式課程</b-link>
          </details>
        </div>
      </b-sidebar>
      <b-card header="篩選器" style="text-align:left;">
        <b-card-text>
          <b-row v-for="(i,j) in choice" :key="j" style="margin-bottom: 5px;">
            <b-col cols="2">
              <center>
              <strong>
                {{j}} : 
              </strong>
              </center>
            </b-col>
            <b-col>
                <div v-for="(k,l) in i" :key="l" :class="get_choice_class(j,l)" @click="chose(j,l)">{{l}}</div><br>
            </b-col>
          </b-row>
        
        </b-card-text>
      </b-card>
    </div>
    <div v-if="DEMO=='DEMO 1'">
    <videoplayer @timechange="updtime"></videoplayer>
    <br>
    <b-button variant="danger" @click="tag('critical')">critical</b-button> &nbsp;
    <b-button variant="warning" @click="tag('moderate')">moderate</b-button> &nbsp;
    <b-button variant="info" @click="tag('info')">info</b-button>
    <br>
    <chart></chart>
    </div>
   <div v-if="DEMO=='DEMO 3'">
     <h1>學生端</h1>
     <demo3></demo3>
   </div>
   <div v-if="DEMO=='DEMO 4'">
     <h1>老師端</h1>
     <demo4></demo4>
   </div>
   <demo5 v-if="DEMO=='DEMO 5'"></demo5>
  </b-container>
  </div>
  
</template>

<script>

import Swal from 'sweetalert2';

import videoplayer from './components/videoplayer.vue';
import chart from './components/chart.vue';
import demo3 from './components/demo3.vue';
import demo4 from './components/demo4.vue';
import demo5 from './components/demo5.vue';

export default {
  name: 'App',
  components:{
    videoplayer,
    chart,
    demo3,
    demo4,
    demo5
  },
  data(){
    return{
      time:0,
      DEMO:"DEMO 5",
      info:"",
      choice:{"級別":{"學校":0,"縣市":0,"全國":0,"國際":0},"教育階段":{"國小":0,"國中":0,"高中":0,"高職":0,"大學":0,"大專":0},"性別":{"男":0,"女":0,"其他":0},"地區":{"南部":0,"東部":0,"北部":0,"中部":0},"領域":{"國語文":0,"英文":0,"數學":0,"自然":0,"社會":0,"體育":0,"辯論":0,"程式設計":0,"音樂":0,"其他":0},"比賽":{}},
    }
  },
  methods:{
    isMobile() {
      if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        return true
      } else {
        return false
      }
    },
    checkonsite(){
      if(document.hidden && this.DEMO==="DEMO 1"){
        Swal.fire('歐不','請不要離開','error');
      }
    },
    tag(arg){
      Swal.fire({
        title:arg,
        text:'第 '+this.time.toFixed(0)+' 秒',
        input: "text",
        showCancelButton: true,
        inputPlaceholder: '請敘述'
        });
    },
    updtime(time){
      this.time=time;
    },
    chgdemo(demo){
      this.DEMO=demo;
    },
    get_choice_class(i,j){
      if(this.choice[i][j]==0){
        return "choice";
      }else{
        return "choice selected";
      }
    },
    chose(i,j){
      if(this.choice[i][j]==0){
        this.choice[i][j]=1;
      }else{
        this.choice[i][j]=0;
      }
    }
  },
  created(){
    if(this.isMobile()){
      Swal.fire('Error','mobile device is not supported yet','error')
      alert('mobile device is not supported yet');
      
      window.location="/mobile.html";
    }
  },
  mounted(){
    setInterval(this.checkonsite,100);
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
}
.sidebarbtn{
  position:absolute;
  left:0vw;
}
.choice{
  display: inline;
  padding-left: 5px;
  padding-right: 5px;
  padding-top: 1px;
  padding-bottom: 1px;
  border-radius: 10px;
  margin-left: 10px;
}
.choice.selected{
  background-color: rgb(182, 181, 181);
  color: white;
}
</style>
