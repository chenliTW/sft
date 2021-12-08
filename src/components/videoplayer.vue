<template>
    <div id="player">
        <center>
            請輸入影片ID：<input v-model="id" v-on:change="load"/>
            <br><br>
            <!--<p>{{id}}</p>-->
        </center>

        <b-row>
            <b-col cols="8">
                <div id="yt-player" style="aspect-ratio: 16 / 9;"></div>
            </b-col>
            <b-col cols="4">
                <b-card :header="`video id: `+id">
                    <b-list-group flush class="list">
                        <b-list-group-item><b-link @click="seekto(0,5)">[00:05]</b-link> 看不懂</b-list-group-item>
                        <b-list-group-item><b-link @click="seekto(15,43)">[15:43]</b-link> 蛤</b-list-group-item>
                        <b-list-group-item v-for="i in 100" :key="i"><b-link @click="seekto(16,32)">[16:32]</b-link> <img height="70px" v-if="i%5==0 || i==2" @click="easteregg()" src="https://popcat.click/img/op.353767c3.png"><img height="70px" v-if="i%5 && i!=2" @click="easteregg()" src="https://popcat.click/img/p.1e9d00be.png"></b-list-group-item>
                    </b-list-group>
                </b-card>
            </b-col>
        </b-row>
        <div v-show="eastertime">
            <img v-for="i in 100" :key="i" height="100px" ref="cat" style="position:absolute;" src="https://popcat.click/img/p.1e9d00be.png">
        </div>
    </div>
</template>
<script>
    import YouTubePlayer from 'youtube-player';
    import Swal from 'sweetalert2';

    export default {
        name:'player',
        data () {
            return {
                id:'M7lc1UVf-VE',
                player:'',
                time:0,
                eastertime:0,
                visited_time:0
            }
        },
        methods:{
            load(){
                this.player.loadVideoById(this.id,0);
                this.time=0;
            },
            timecheck(){
                this.player.getCurrentTime().then((realtime)=>{
                    this.player.getPlaybackRate().then((rate)=>{
                        if(realtime-this.visited_time>rate+0.1){
                            console.log("[-] detected jump ,falling back to "+this.time.toFixed(1)+" sec");
                            if(this.time!==0){
                                Swal.fire('歐不','你還沒看過不能跳到那裡','error');
                            }
                            this.player.seekTo(this.time);
                        }else{
                            this.time=realtime;
                            if(this.time>this.visited_time){
                                this.visited_time=this.time;
                            }
                            this.$emit("timechange",this.time);
                        }
                    });
                });
            },
            seekto(min,sec){
                let t=min*60+sec;
                if(t<=this.visited_time){
                    this.time=t;
                    this.player.seekTo(t);
                }else{
                    Swal.fire('歐不','你還沒看過不能跳到那裡','error');
                }
            },
            gettime(){
                return this.time;
            },
            easteregg(){
                //this.id="Otnhzbhf3ps";
                //this.time=0;
                //this.load();

                /*
                disable this because it is too gross
                this.eastertime=1;
                this.$refs.cat.forEach(cat => {
                    cat.style.left = Math.random() * (window.innerWidth) + 'px'
                    cat.style.top = Math.random() * (window.innerHeight) + 'px'
                })
                */
            }
        },
        mounted(){
            if(location.hash.length){
                this.id=location.hash.substring(1,);
                location.hash="";
            }
            this.player=YouTubePlayer('yt-player',{
                width: '100%',
                height: '*',
            });
            this.load();
            setInterval(this.timecheck,1000);
        }
        
    };
</script>

<style scoped>
    .list{
        text-align: left;
        aspect-ratio: 8 / 9;
        overflow-y: scroll;
    }
    .video{
        width: 10px;
    }
</style>