<template>
    <div id="demo4">
        <h3>老師彈跳</h3>
        <b-button @click="open('pop')">沒開mic</b-button><br><br>
        <b-button @click="open('not')" v-b-modal.modal-87>系統通知</b-button><br><br>
        <b-modal id="modal-87" title="問題" hide-footer> 
            抱歉沒聽清楚,老師剛剛說神摸?
        </b-modal>
        <hr>
        <h3>老師提問</h3>
        <b-button @click="open('lst')">問題集</b-button><br><br>
        <b-button v-b-modal.modal-1>製作quiz</b-button><br><br>
        <b-modal id="modal-1" title="新增問題" hide-footer>
            <b-row>
                <b-col cols="9">
                    <h4 style="display: inline;">製作或彈跳 quiz:</h4>
                    <b-form-input></b-form-input>
                    <br>
                    <draggable tag="ul" class="list-group" handle=".handle">
                        <li class="list-group-item" v-for="(question,idx) in questions" :key="question.title">
                            <b-row>
                            <b-col cols="1">
                                <font-awesome-icon :icon="['fas','grip-horizontal']" class="handle" />  
                            </b-col>
                            <b-col>
                                <b-form-input size="sm" :value="question.title">
                                </b-form-input>
                            </b-col>
                            <b-col cols="2">
                                <div @click="del(idx,-1)">
                                    <font-awesome-icon :icon="['fas','trash-alt']" class="handle" /> 
                                </div>
                            </b-col>
                            </b-row>
                            <div v-if="question.type=='sin'">
                                <input :ref="`inp`+idx" type="file" accept="image/*" @change="upload(event,idx)">
                                <img :ref="`img`+idx"/>
                                請勾選正確答案(單選):
                                <b-form-radio-group>
                                <draggable>
                                <div v-for="(opt,opt_idx) in question.options" :key="opt" class="handle">
                                    <b-row>
                                        <b-col cols="1">
                                            <font-awesome-icon :icon="['fas','grip-lines']" class="handle" /> 
                                        </b-col>
                                        <b-col cols="8">
                                            <b-form-radio>
                                                <b-form-input size="sm" :value="opt.text" >
                                                </b-form-input>
                                            </b-form-radio>
                                        </b-col>
                                        <b-col cols="1">
                                            <font-awesome-icon :icon="['fas','times']" class="handle" @click="del(idx,opt_idx)" /> 
                                        </b-col>
                                    </b-row>
                                </div>
                                </draggable>
                                </b-form-radio-group>
                                <div>
                                <font-awesome-icon :icon="['fas','plus']" @click="add_opt(idx)"/> 加選項
                                </div>
                            </div>
                            <div v-if="question.type=='mul'">
                                <input :ref="`inp`+idx" type="file" accept="image/*" @change="upload(event,idx)">
                                <img :ref="`img`+idx"/>
                                請勾選正確答案(多選):
                                <draggable>
                                <div v-for="(opt,opt_idx) in question.options" :key="opt" class="handle">
                                    <b-row>
                                        <b-col cols="1">
                                            <font-awesome-icon :icon="['fas','grip-lines']" class="handle" /> 
                                        </b-col>
                                        <b-col cols="8">
                                            <b-form-checkbox >
                                                <b-form-input size="sm" :value="opt.text">
                                                </b-form-input>
                                            </b-form-checkbox>
                                        </b-col>
                                        <b-col cols="1">
                                            <font-awesome-icon :icon="['fas','times']" class="handle" @click="del(idx,opt_idx)" /> 
                                        </b-col>
                                    </b-row>
                                </div>
                                </draggable>
                                <div>
                                <font-awesome-icon :icon="['fas','plus']" @click="add_opt(idx)"/> 加選項
                                </div>
                            </div>
                            <div v-if="question.type=='que'">
                                <input :ref="`inp`+idx" type="file" accept="image/*" @change="upload(event,idx)">
                                <img :ref="`img`+idx"/>
                                請輸入正確答案:
                                <b-form-input size="sm">
                                </b-form-input>
                            </div>
                        </li>
                    </draggable>
                </b-col>
                <b-col>
                    <br>
                    <br>
                    <b-button @click="add('question')">問答</b-button><br><br>
                    <b-button @click="add('sinchoice')">單選</b-button><br><br>
                    <b-button @click="add('mulchoice')">多選</b-button><br><br>
                </b-col>
            </b-row>   
            <div class="modal-footer">
                <b-button @click="$bvModal.hide('modal-1')">取消</b-button>
                <b-button @click="$bvModal.hide('modal-1')">儲存</b-button>
                <b-button @click="$bvModal.hide('modal-1')">彈跳</b-button>
            </div>
        </b-modal>
        <b-button v-b-modal.modal-2 >彈跳問答題</b-button><br><br>
        <b-modal id="modal-2" title="彈跳問答題">
            <center>
                    <b-button @click="cho();mov();" :variant="good">A</b-button> <b-button @click="cho()" :variant="bad">B</b-button> <b-button @click="cho()" :variant="bad">C</b-button> <b-button @click="cho()" :variant="bad">D</b-button>
            </center>
            <br>
            <div v-if="good=='success'">
            <b-row>
                <b-col>
                    <table border="1">
                        <tr v-for="i in [0,5,15,20,25]" :key="i">
                            <td v-for="j in 5" :key="j">
                                <div v-if="i+j<=stat && i+j!=1 && i+j!=5" style="background-color: lawngreen;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div>
                                <div v-if="i+j>stat"  style="background-color: gray;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div>
                                <div v-if="i+j<=stat && (i+j==1 || i+j==5)"  style="background-color: red;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div>
                            </td>
                        </tr>
                    </table>
                </b-col>
                <b-col>
                    <b-list-group>
                    <b-list-group-item v-if="stat>=1">[1] 立法院</b-list-group-item>
                    <b-list-group-item v-if="stat>=2">[2] 行政院</b-list-group-item>
                    <b-list-group-item v-if="stat>=3">[3] 行政院</b-list-group-item>
                    <b-list-group-item v-if="stat>=4">[4] 行政院</b-list-group-item>
                    <b-list-group-item v-if="stat>=5">[5] 監察院</b-list-group-item>
                    </b-list-group>
                </b-col>
            </b-row>
            <a href="/score.csv" download id="download" hidden ref="down"></a>
            <b-button class="mt-3" variant="outline-info" block @click="download()">下載成績表(csv)</b-button>
            </div>
        </b-modal>
        <b-button v-b-modal.modal-3 @click="mov()">彈跳選擇題</b-button><br><br>
        <b-modal id="modal-3" title="彈跳選擇題" hide-footer>
            <b-row>
                <b-col>
                    <table border="1">
                        <tr v-for="i in [0,5,15,20,25]" :key="i"><td v-for="j in 5" :key="j"><div v-if="i+j<=stat" style="background-color: lawngreen;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div><div v-if="i+j>stat"  style="background-color: hotpink;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div></td></tr>
                    </table>
                    <center>
                        <br>
                        <br>
                    <b-button @click="$bvModal.show('modal-3-1');render+=1;">查看答題分佈</b-button>
                    </center>
                    <b-modal id="modal-3-1" title="答題分佈">
                        <demo4chart :ren="render"></demo4chart>
                    </b-modal>
                </b-col>
                <b-col>
                    <b-list-group>
                    <b-list-group-item v-if="stat>=1">[1] A</b-list-group-item>
                    <b-list-group-item v-if="stat>=2">[2] B</b-list-group-item>
                    <b-list-group-item v-if="stat>=3">[3] A</b-list-group-item>
                    <b-list-group-item v-if="stat>=4">[4] C</b-list-group-item>
                    <b-list-group-item v-if="stat>=5">[5] D</b-list-group-item>
                    </b-list-group>
                </b-col>
                <a href="/score.csv" download id="download" hidden ref="down"></a>
                <b-button class="mt-3" variant="outline-info" block @click="download()">下載成績表(csv)</b-button>
            </b-row>
            <div class="modal-footer">
                <b-button variant="primary">揭露答題分佈</b-button>
                <b-button variant="danger" @click="$bvModal.hide('modal-3')">結束</b-button>
            </div>
        </b-modal>
        <b-button v-b-modal.modal-3 @click="mov()">彈跳quiz</b-button><br><br>
        <b-modal id="modal-3" title="彈跳quiz" hide-footer>
            <b-row>
                <b-col>
                    <table border="1">
                        <tr v-for="i in [0,5,15,20,25]" :key="i"><td v-for="j in 5" :key="j"><div v-if="i+j<=stat" style="background-color: lawngreen;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div><div v-if="i+j>stat"  style="background-color: hotpink;">&nbsp;&nbsp;{{i+j}}&nbsp;&nbsp;</div></td></tr>
                    </table>
                    <center>
                        <br>
                        <br>
                    <b-button @click="$bvModal.show('modal-3-1');render+=1;">查看答題分佈</b-button>
                    </center>
                    <b-modal id="modal-3-1" title="答題分佈">
                        <demo4chart :ren="render"></demo4chart>
                    </b-modal>
                </b-col>
                <b-col>
                    <b-list-group>
                    <b-list-group-item v-if="stat>=1">[1] 已交</b-list-group-item>
                    <b-list-group-item v-if="stat>=2">[2] 已交</b-list-group-item>
                    <b-list-group-item v-if="stat>=3">[3] 已交</b-list-group-item>
                    <b-list-group-item v-if="stat>=4">[4] 已交</b-list-group-item>
                    <b-list-group-item v-if="stat>=5">[5] 已交</b-list-group-item>
                    </b-list-group>
                </b-col>
                <a href="/score.csv" download id="download" hidden ref="down"></a>
                <b-button class="mt-3" variant="outline-info" block @click="download()">下載成績表(csv)</b-button>
            </b-row>
            <div class="modal-footer">
                <b-button variant="primary">揭露答題分佈</b-button>
                <b-button variant="danger" @click="$bvModal.hide('modal-3')">結束</b-button>
            </div>
        </b-modal>
        <b-button v-b-modal.modal-4>彈跳檔案</b-button><br><br>
        <b-modal id="modal-4" title="彈跳檔案" hide-footer>
             <b-form-file
            v-model="file1"
            :state="Boolean(file1)"
            placeholder="Choose a file or drop it here..."
            drop-placeholder="Drop file here..."
            @input="calmd5()"
            ></b-form-file>
            md5: {{md5}}
            <b-button class="mt-3" variant="outline-danger" block @click="$bvModal.hide('modal-4')">彈跳</b-button>
        </b-modal>
    </div>
</template>
<script>

import draggable from 'vuedraggable';
import CryptoJS from 'crypto-js';
import demo4chart from './demo4chart.vue';

var reader = new FileReader();

export default {
    name:"demo4",
    components:{
        draggable,
        demo4chart
    },
    data(){
        return {
            questions:[
                ],
            url:"",
            file1:"",
            md5:"",
            render:0,
            stat:0,
            good:"",
            bad:"",
        }
    },
    created(){
            Notification.requestPermission();
            reader.onloadend=(evt)=>{
                //console.log("md5 oao");
                if (evt.target.readyState == FileReader.DONE) {
                    this.md5=CryptoJS.MD5(evt.target.result).toString();
                }
            }
    },
    methods:{
        cho(){
            this.good="success";
            this.bad="danger";
        },
        download(){
            this.$refs["down"].click();
            console.log("down");
        },
        mov(){
            setTimeout(()=>{this.stat+=1},3500);
            setTimeout(()=>{this.stat+=1},4000);
            setTimeout(()=>{this.stat+=1},4150);
            setTimeout(()=>{this.stat+=1},4200);
            setTimeout(()=>{this.stat+=1},5000);
        },
        calmd5(){
            reader.readAsBinaryString(this.file1);
        },
        add(type){
            if(type=="question"){
                this.questions.push({title:"",type:"que"})
            }else if(type=="sinchoice"){
                this.questions.push({title:"",type:"sin",options:[]})
            }else if(type=="mulchoice"){
                this.questions.push({title:"",type:"mul",options:[]})  
            }
        },
        add_opt(idx){
            this.questions[idx].options.push({text:'',selected:false})
        },
        del(idx,chi){
            if(chi===-1){
                this.questions.splice(idx,1);
            }else{
                this.questions[idx].options.splice(chi,1);
            }
        },
        open(title){
            
            if(title=="pop"){
                let win=window.open('', '_blank', 'directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=500,height=220');
                let html=`<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>緊急</h1>
                
                <br>
                <h1 class="text-danger">緊急訊息</h1>
                <button type="button" class="btn btn-danger" >mute</button>
                </center>
                </body>`.replace("</s>","</scr"+"ipt>").replace("</s>","</scr"+"ipt>");
                win.document.write(html); 
            }else if(title=='not'){
                if (Notification.permission === "granted") {
                    new Notification("這是系統通知");
                }else{
                    Notification.requestPermission();
                }
            }else if(title=='lst'){
                let win=window.open('', '_blank', 'directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=500');
                let html=`<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>問題集</h1>
                </center>
                <ul class="list-group" id="2">
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">美國蘋果公司選擇在中國組裝零件為成品，主要是考量下列哪
 個工業區位？ (Ａ)原料 (Ｂ)市場 (Ｃ)勞工 (Ｄ)動力。</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">下列哪個選項的字形完全正確？﹝A﹞他自認為超級鎖售員，不滿加薪未果，洩露公司機密，公司決定撤
宵他職務，對他提出告訴﹝B﹞現今詐騙集團猖獗，詐騙案層出不窮，立委倡議加重刑責以遏止犯罪﹝C﹞小說
中常有主角跌落懸崖後發現絕世劍訣，每日通霄達旦苦練，只需一個念頭，寶劍便會自動出峭﹝D﹞他將積畜投
入牧場，不料因病毒感染，大量牲搐死亡，損失慘重。</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">「與荒野相遇，我其實感受的是一種生活，一種與自然澈底相融的生活……每一時的鼻息耳目都浸潤在自
然裡。」請問作者的境界較近似於哪一個選項？﹝A﹞知之者﹝B﹞好之者﹝C﹞樂之者﹝D﹞以上皆非。</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">The roses _____ on the table will be given to Cindy as a birthday gift.
(A) that is (B) who are (C) X (D) which is</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">. I haven’t been to the movies these days. Are there any good _____ this week? (101 基測)
(A) ones (B) others (C) them (D) those</li>
                </ul>
                <center>
                <br>
                <button type="button" class="btn btn-primary" onclick="deletes()">刪除</button>
                <button type="button" class="btn btn-primary" onclick="selectall()">全選</button><br><br>
                <button type="button" class="btn btn-primary" onclick="deletes()">彈跳</button>
                </center>
                <script>
                let ele=document.getElementById("2");
                function deletes(){
                    for(let i in ele.children){
                        if(ele.children[i].children[0].checked==1){
                            delete ele.children[i].remove();
                        }
                    }
                }
                function selectall(){
                    for(let i in ele.children){
                        ele.children[i].children[0].checked=1;
                    }
                }
                function tmp(){
                    let win=window.open('', '_blank', 'directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=200');
                    let html='<head><title>tmp</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                    <body>\
                    <center>\
                    <h1>彈跳</h1>\
                    <ul class="list-group">\
                        <center>\
                        <li class="list-group-item"><img height="70px" src="https://popcat.click/img/p.1e9d00be.png" onclick="tmp()"></li>\
                        <li class="list-group-item"><textbox>Q (A) OAO</textbox></li>\
                        </center>\
                    </ul>\
                    <center>\
                    <br>\
                    </center>\
                    </body>';
                    win.document.write(html); 
                }
                </s>
                </body>`.replace("</s>","</scr"+"ipt>").replace("</s>","</scr"+"ipt>");
                win.document.write(html); 
            }
        }
    }
}
</script>

<style scoped>
.oao{
    background-color: hotpink;
    
}
</style>
