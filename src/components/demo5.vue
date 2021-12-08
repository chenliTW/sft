<template>
  <div id="demo5">
    <b-row>
      <b-col>
        <b-card title="老師端">
          <hr />
          <b-row>
            <b-col cols="1"></b-col>
            <b-col>
              數學系老師 白居易<br /><b-badge pill variant="secondary"
                >1234567@zbridge.lwet.sft.tw</b-badge
              ><br />
            </b-col>
            <b-col>
              <b-alert show :variant="block == 'success' ? 'danger' : 'success'"
                ><div v-if="this.block == 'success'" style="display: inline">
                  阻擋
                </div>
                <div v-else style="display: inline">接受</div>
                提問中</b-alert
              >
            </b-col>
            <b-col cols="1"></b-col>
          </b-row>
          
          <b-button
            :variant="block"
            @click="
              () => {
                if (this.block == 'success') {
                  this.block = 'danger';
                } else {
                  this.block = 'success';
                  this.asked_question=[]; 
                }asked_question_reset();
              }
            "
            >開始
            <div v-if="this.block == 'success'" style="display: inline">
              接受
            </div>
            <div v-else style="display: inline">阻擋</div>
            提問</b-button
          >
          <br /><br />
          <div v-if="this.block!='success'">
          <b-list-group-item v-bind:key="i" v-for="i in asked_question"> {{i}}</b-list-group-item> 
          </div>
          <br />
          <b-button variant="danger" v-b-modal.modal-81>緊急彈跳授權</b-button>
          <b-modal id="modal-81" title="緊急彈跳授權" hide-footer>
            <h5>授權對象:</h5>
            <b-form-input v-model="ema"></b-form-input>
            <center>
              <br />
              <b-button @click="addemail()">新增</b-button>
            </center>
            <br />
            <b-list-group>
              <b-list-group-item :key="i" v-for="i in email">
                {{ i }}
              </b-list-group-item>
            </b-list-group>
          </b-modal>
          <hr />
          <b-button v-b-modal.modal-2 >彈跳問答題</b-button><br /><br />
          <b-modal id="modal-2" title="彈跳問答題" @show="mov();">
            <center>
              <!--<b-button
                @click="
                  cho();
                  mov();
                "
                :variant="good"
                >A</b-button
              >
              <b-button @click="cho()" :variant="bad">B</b-button>
              <b-button @click="cho()" :variant="bad">C</b-button>
              <b-button @click="cho()" :variant="bad">D</b-button>-->
            </center>
            <br />
            <div v-if="1">
              <b-row>
                <b-col>
                  <table border="1">
                    <tr v-for="i in [0, 5, 15, 20, 25]" :key="i">
                      <td v-for="j in 5" :key="j">
                        <div
                          v-if="i + j <= stat && i + j != 1 && i + j != 5"
                          style="background-color: lawngreen"
                        >
                          &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                        </div>
                        <div v-if="i + j > stat" style="background-color: gray">
                          &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                        </div>
                        <div
                          v-if="i + j <= stat && (i + j == 1 || i + j == 5)"
                          style="background-color: lawngreen"
                        >
                          &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                        </div>
                      </td>
                    </tr>
                  </table>
                </b-col>
                <b-col>
                  <b-list-group>
                    <b-list-group-item v-if="stat >= 1"
                      >[1] 立法院</b-list-group-item
                    >
                    <b-list-group-item v-if="stat >= 2"
                      >[2] 行政院</b-list-group-item
                    >
                    <b-list-group-item v-if="stat >= 3"
                      >[3] 行政院</b-list-group-item
                    >
                    <b-list-group-item v-if="stat >= 4"
                      >[4] 行政院</b-list-group-item
                    >
                    <b-list-group-item v-if="stat >= 5"
                      >[5] 監察院</b-list-group-item
                    >
                  </b-list-group>
                </b-col>
              </b-row>
              <a href="/score.csv" download id="download" hidden ref="down"></a>
              <b-button
                class="mt-3"
                variant="outline-info"
                block
                @click="download()"
                >下載成績表(csv)</b-button
              >
            </div>
          </b-modal>
          <b-button v-b-modal.modal-3 @click="mov()">彈跳選擇題</b-button
          ><br /><br />
          <b-modal id="modal-3" title="彈跳選擇題" hide-footer>
            <b-row>
              <b-col>
                <table border="1">
                  <tr v-for="i in [0, 5, 15, 20, 25]" :key="i">
                    <td v-for="j in 5" :key="j">
                      <div
                        v-if="i + j <= stat"
                        style="background-color: lawngreen"
                      >
                        &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                      </div>
                      <div
                        v-if="i + j > stat"
                        style="background-color: gray"
                      >
                        &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                      </div>
                    </td>
                  </tr>
                </table>
                <center>
                  <br />
                  <br />
                  <b-button
                    @click="
                      $bvModal.show('modal-3-1');
                      render += 1;
                    "
                    >查看答題分佈</b-button
                  >
                </center>
                <b-modal id="modal-3-1" title="答題分佈">
                  <demo4chart :ren="render"></demo4chart>
                </b-modal>
              </b-col>
              <b-col>
                <b-list-group>
                  <b-list-group-item v-if="stat >= 1">[1] A</b-list-group-item>
                  <b-list-group-item v-if="stat >= 2">[2] B</b-list-group-item>
                  <b-list-group-item v-if="stat >= 3">[3] A</b-list-group-item>
                  <b-list-group-item v-if="stat >= 4">[4] C</b-list-group-item>
                  <b-list-group-item v-if="stat >= 5">[5] D</b-list-group-item>
                </b-list-group>
              </b-col>
              <a href="/score.csv" download id="download" hidden ref="down"></a>
              <b-button
                class="mt-3"
                variant="outline-info"
                block
                @click="download()"
                >下載成績表(csv)</b-button
              >
            </b-row>
            <div class="modal-footer">
              <b-button variant="primary">揭露答題分佈</b-button>
              <b-button variant="danger" @click="$bvModal.hide('modal-3')"
                >結束</b-button
              >
            </div>
          </b-modal>
          <b-button v-b-modal.modal-1>Quiz</b-button><br /><br />
          <b-modal id="modal-1" title="新增問題" hide-footer>
            <b-row>
              <b-col cols="9">
                <h4 style="display: inline">製作或彈跳 quiz:</h4>
                <b-form-input></b-form-input>
                <br />
                <draggable tag="ul" class="list-group" handle=".handle">
                  <li
                    class="list-group-item"
                    v-for="(question, idx) in questions"
                    :key="question.title"
                  >
                    <b-row>
                      <b-col cols="1">
                        <font-awesome-icon
                          :icon="['fas', 'grip-horizontal']"
                          class="handle"
                        />
                      </b-col>
                      <b-col>
                        <b-form-input size="sm" :value="question.title">
                        </b-form-input>
                      </b-col>
                      <b-col cols="2">
                        <div @click="del(idx, -1)">
                          <font-awesome-icon
                            :icon="['fas', 'trash-alt']"
                            class="handle"
                          />
                        </div>
                      </b-col>
                    </b-row>
                    <div v-if="question.type == 'sin'">
                      <input
                        :ref="`inp` + idx"
                        type="file"
                        accept="image/*"
                        @change="upload(event, idx)"
                      />
                      <img :ref="`img` + idx" />
                      請勾選正確答案(單選):
                      <b-form-radio-group>
                        <draggable>
                          <div
                            v-for="(opt, opt_idx) in question.options"
                            :key="opt"
                            class="handle"
                          >
                            <b-row>
                              <b-col cols="1">
                                <font-awesome-icon
                                  :icon="['fas', 'grip-lines']"
                                  class="handle"
                                />
                              </b-col>
                              <b-col cols="8">
                                <b-form-radio>
                                  <b-form-input size="sm" :value="opt.text">
                                  </b-form-input>
                                </b-form-radio>
                              </b-col>
                              <b-col cols="1">
                                <font-awesome-icon
                                  :icon="['fas', 'times']"
                                  class="handle"
                                  @click="del(idx, opt_idx)"
                                />
                              </b-col>
                            </b-row>
                          </div>
                        </draggable>
                      </b-form-radio-group>
                      <div>
                        <font-awesome-icon
                          :icon="['fas', 'plus']"
                          @click="add_opt(idx)"
                        />
                        加選項
                      </div>
                    </div>
                    <div v-if="question.type == 'mul'">
                      <input
                        :ref="`inp` + idx"
                        type="file"
                        accept="image/*"
                        @change="upload(event, idx)"
                      />
                      <img :ref="`img` + idx" />
                      請勾選正確答案(多選):
                      <draggable>
                        <div
                          v-for="(opt, opt_idx) in question.options"
                          :key="opt"
                          class="handle"
                        >
                          <b-row>
                            <b-col cols="1">
                              <font-awesome-icon
                                :icon="['fas', 'grip-lines']"
                                class="handle"
                              />
                            </b-col>
                            <b-col cols="8">
                              <b-form-checkbox>
                                <b-form-input size="sm" :value="opt.text">
                                </b-form-input>
                              </b-form-checkbox>
                            </b-col>
                            <b-col cols="1">
                              <font-awesome-icon
                                :icon="['fas', 'times']"
                                class="handle"
                                @click="del(idx, opt_idx)"
                              />
                            </b-col>
                          </b-row>
                        </div>
                      </draggable>
                      <div>
                        <font-awesome-icon
                          :icon="['fas', 'plus']"
                          @click="add_opt(idx)"
                        />
                        加選項
                      </div>
                    </div>
                    <div v-if="question.type == 'que'">
                      <input
                        :ref="`inp` + idx"
                        type="file"
                        accept="image/*"
                        @change="upload(event, idx)"
                      />
                      <img :ref="`img` + idx" />
                      請輸入正確答案:
                      <b-form-input size="sm"> </b-form-input>
                    </div>
                  </li>
                </draggable>
              </b-col>
              <b-col>
                <br />
                <br />
                <b-button @click="add('question')">問答</b-button><br /><br />
                <b-button @click="add('sinchoice')">單選</b-button><br /><br />
                <b-button @click="add('mulchoice')">多選</b-button><br /><br />
              </b-col>
            </b-row>
            <div class="modal-footer">
              <b-button @click="$bvModal.hide('modal-1')">取消</b-button>
              <b-button @click="$bvModal.hide('modal-1')">儲存</b-button>
              <b-button @click="$bvModal.hide('modal-1')">彈跳</b-button>
            </div>
          </b-modal>
          <!--<b-button v-b-modal.modal-3 @click="mov()">彈跳quiz</b-button
          ><br /><br />-->
          <b-modal id="modal-3-7" title="彈跳quiz" hide-footer>
            <b-row>
              <b-col>
                <table border="1">
                  <tr v-for="i in [0, 5, 15, 20, 25]" :key="i">
                    <td v-for="j in 5" :key="j">
                      <div
                        v-if="i + j <= stat"
                        style="background-color: lawngreen"
                      >
                        &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                      </div>
                      <div
                        v-if="i + j > stat"
                        style="background-color: hotpink"
                      >
                        &nbsp;&nbsp;{{ i + j }}&nbsp;&nbsp;
                      </div>
                    </td>
                  </tr>
                </table>
                <center>
                  <br />
                  <br />
                  <b-button
                    @click="
                      $bvModal.show('modal-3-1');
                      render += 1;
                    "
                    >查看答題分佈</b-button
                  >
                </center>
                <b-modal id="modal-3-1" title="答題分佈">
                  <demo4chart :ren="render"></demo4chart>
                </b-modal>
              </b-col>
              <b-col>
                <b-list-group>
                  <b-list-group-item v-if="stat >= 1"
                    >[1] 已交</b-list-group-item
                  >
                  <b-list-group-item v-if="stat >= 2"
                    >[2] 已交</b-list-group-item
                  >
                  <b-list-group-item v-if="stat >= 3"
                    >[3] 已交</b-list-group-item
                  >
                  <b-list-group-item v-if="stat >= 4"
                    >[4] 已交</b-list-group-item
                  >
                  <b-list-group-item v-if="stat >= 5"
                    >[5] 已交</b-list-group-item
                  >
                </b-list-group>
              </b-col>
              <a href="/score.csv" download id="download" hidden ref="down"></a>
              <b-button
                class="mt-3"
                variant="outline-info"
                block
                @click="download()"
                >下載成績表(csv)</b-button
              >
            </b-row>
            <div class="modal-footer">
              <b-button variant="primary">揭露答題分佈</b-button>
              <b-button variant="danger" @click="$bvModal.hide('modal-3')"
                >結束</b-button
              >
            </div>
          </b-modal>
          <b-button v-b-modal.modal-4>彈跳檔案</b-button><br /><br />
          <b-modal id="modal-4" title="彈跳檔案" hide-footer>
            <b-form-file
              v-model="file1"
              :state="Boolean(file1)"
              placeholder="Choose a file or drop it here..."
              drop-placeholder="Drop file here..."
              @input="calmd5()"
            ></b-form-file>
            md5: {{ md5 }}
            <b-button
              class="mt-3"
              variant="outline-danger"
              block
              @click="$bvModal.hide('modal-4')"
              >彈跳</b-button
            >
          </b-modal>
          <hr />
          <b-button @click="open('lst')">問題集</b-button><br /><br />
          <b-button v-b-modal.modal-51>檔案集</b-button>
        </b-card>
      </b-col>
      <b-col>
        <b-card title="學生端">
          <hr />
          <b-row>
            <b-col>
              物理學系 一年級 B10111001 王小明<br /><b-badge
                pill
                variant="secondary"
                >b10111001@zbridge.lwet.sft.tw</b-badge
              ><br />
            </b-col>
          </b-row>
          <div>
            <br />
            <b-button @click="open('cap')">截圖提問</b-button><br /><br />
            <b-button @click="open('ask')">文字提問</b-button><br /><br />
            <b-button variant="danger" @click="open('emg')">緊急彈跳</b-button>
             
            <!--<b-button v-b-modal.modal-3 @click="reg()">回答問題</b-button
            ><br /><br />
            <b-modal id="modal-3" title="回答問題" hide-footer>
              美國蘋果公司選擇在中國組裝零件為成品，主要是考量下列哪
 個工業區位？ (Ａ)原料 (Ｂ)市場 (Ｃ)勞工 (Ｄ)動力。<br />
              <center>
                <br />
                <b-button @click="cho()" :variant="good">A</b-button>
                <b-button @click="cho()" :variant="bad">B</b-button>
                <b-button @click="cho()" :variant="bad">C</b-button>
                <b-button @click="cho()" :variant="bad">D</b-button>
              </center>
              <b-button
                class="mt-3"
                variant="outline-danger"
                block
                @click="$bvModal.hide('modal-4')"
                >結束</b-button
              >
              <b-button
                v-if="ok"
                class="mt-3"
                variant="outline-primary"
                block
                @click="
                  $bvModal.show('modal-3-1');
                  render += 1;
                "
                >查看答題分佈</b-button
              >
              <b-modal id="modal-3-1" title="答題分佈">
                <demo4chart :ren="render"></demo4chart>
              </b-modal>
            </b-modal>

            <b-button v-b-modal.modal-9>回答quiz</b-button><br /><br />
            <b-modal id="modal-9" title="回答quiz" hide-footer>
              <h3>quiz title</h3>
              <center>
                <h3>
                  <div v-if="fin" style="color: green">分數：2/3</div>
                </h3>
              </center>
              <b-list-group>
                <b-list-group-item :style="quiz_c"
                  >1. 問答題<br /><b-form-input></b-form-input
                ></b-list-group-item>
                <b-list-group-item :style="quiz_c">
                  2. 單選題<br /><b-form-radio-group
                    ><b-form-radio>正確答案</b-form-radio><br /><b-form-radio
                      >錯誤答案</b-form-radio
                    ></b-form-radio-group
                  ></b-list-group-item
                >
                <b-list-group-item :style="quiz_w"
                  >3. 多選題<br /><b-form-checkbox>正確</b-form-checkbox
                  ><b-form-checkbox>錯誤</b-form-checkbox
                  ><b-form-checkbox>正確</b-form-checkbox></b-list-group-item
                >
              </b-list-group>
              <b-button
                class="mt-3"
                variant="outline-danger"
                block
                @click="
                  fin_quiz();
                  reg();
                "
                >送出答案</b-button
              >
              <b-button
                v-if="ok"
                class="mt-3"
                variant="outline-primary"
                block
                @click="
                  $bvModal.show('modal-9-1');
                  render += 1;
                "
                >查看答對率</b-button
              >
              <b-modal id="modal-9-1" title="答對率">
                <demo4chart1 :ren="render"></demo4chart1>
              </b-modal>
            </b-modal>
            
            <b-button @click="open('set')">問題集</b-button><br /><br />
            <b-button variant="danger" @click="open('emg')">緊急彈跳</b-button
            ><br /><br />
            <b-button v-b-modal.modal-4>彈跳檔案</b-button><br /><br />
            <b-modal id="modal-4" title="收到檔案" hide-footer>
              <center>
                <img
                  src="data:image/png;base64,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"
                />
                <br />
                filename: example.txt<br />
                (md5: 63efc1f38cb7aa5a262ea1ecef084057)
              </center>
              <a
                href="/example.txt"
                download
                id="download"
                hidden
                ref="down"
              ></a>
              <b-button
                class="mt-3"
                variant="outline-danger"
                block
                @click="download()"
                >download</b-button
              >
            </b-modal>-->
            <hr />
            <b-button @click="open('lst')">問題集</b-button><br /><br />
            <b-button v-b-modal.modal-51>檔案集</b-button><br /><br />
            <b-modal id="modal-51" title="檔案集" hide-footer>
              <b-list-group>
                <b-list-group-item v-b-modal.modal-52
                  >README.txt</b-list-group-item
                >
                <b-list-group-item v-b-modal.modal-53
                  >tutorial.mp4</b-list-group-item
                >
              </b-list-group>
            </b-modal>
            <b-modal id="modal-52" title="收到檔案" hide-footer>
              <center>
                <img
                  src="data:image/png;base64,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"
                />
                <br />
                filename: README.txt
              </center>
              <a
                href="/README.txt"
                download
                id="download"
                hidden
                ref="down"
              ></a>
              <b-button
                class="mt-3"
                variant="outline-danger"
                block
                @click="download()"
                >download</b-button
              >
            </b-modal>
            <b-modal id="modal-53" title="收到檔案" hide-footer>
              <center>
                <img
                  src="data:image/png;base64,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"
                />
                <br />
                filename: tutorial.mp4
              </center>
              <a
                href="/tutorial.mp4"
                download
                id="download"
                hidden
                ref="down"
              ></a>
              <b-button
                class="mt-3"
                variant="outline-danger"
                block
                @click="download()"
                >download</b-button
              >
            </b-modal>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>
<script>
import draggable from "vuedraggable";
import CryptoJS from "crypto-js";
import demo4chart from "./demo4chart.vue";
//import demo4chart1 from "./demo4chart1.vue";

var reader = new FileReader();

export default {
  name: "demo5",
  components: {
    draggable,
    demo4chart,
    //demo4chart1,
  },
  data() {
    return {
      questions: [],
      url: "",
      file1: "",
      md5: "",
      render: 0,
      stat: 0,
      good: "",
      bad: "",
      ok: 0,
      quiz_c: "",
      quiz_w: "",
      fin: 0,
      block: "success",
      email: [],
      ema: "",
      asked_question:[],
    };
  },
  created() {
    Notification.requestPermission();
    reader.onloadend = (evt) => {
      //console.log("md5 oao");
      if (evt.target.readyState == FileReader.DONE) {
        this.md5 = CryptoJS.MD5(evt.target.result).toString();
      }
    };
  },
  methods: {
    asked_question_reset(){
      this.asked_question=[];
      setTimeout(()=>{this.asked_question.push("物理一 王小明: 老師我想問第三大題第五題")},500);
      setTimeout(()=>{this.asked_question.push("資管一 張大明: 老師今天晚餐吃什摸？")},1000);
      setTimeout(()=>{this.asked_question.push("數學六 汪忠明: 請問番三角函數要考嗎?")},2000);
      setTimeout(()=>{this.asked_question.push("中文二 Adam: I don't know how to solve p.47 Question 2")},3500);
    },cho() {
      this.good = "success";
      this.bad = "danger";
    },
    download() {
      this.$refs["down"].click();
      console.log("down");
    },
    mov() {
      this.stat=0;
      setTimeout(() => {
        this.stat += 1;
      }, 1500);
      setTimeout(() => {
        this.stat += 1;
      }, 2500);
      setTimeout(() => {
        this.stat += 1;
      }, 3050);
      setTimeout(() => {
        this.stat += 1;
      }, 4200);
      setTimeout(() => {
        this.stat += 1;
      }, 5000);
    },
    calmd5() {
      reader.readAsBinaryString(this.file1);
    },
    add(type) {
      if (type == "question") {
        this.questions.push({ title: "", type: "que" });
      } else if (type == "sinchoice") {
        this.questions.push({ title: "", type: "sin", options: [] });
      } else if (type == "mulchoice") {
        this.questions.push({ title: "", type: "mul", options: [] });
      }
    },
    add_opt(idx) {
      this.questions[idx].options.push({ text: "", selected: false });
    },
    del(idx, chi) {
      if (chi === -1) {
        this.questions.splice(idx, 1);
      } else {
        this.questions[idx].options.splice(chi, 1);
      }
    },
    open(title) {
      if (title == "pop") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=500,height=220"
        );
        let html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>緊急</h1>
                
                <br>
                <h1 class="text-danger">緊急訊息</h1>
                <button type="button" class="btn btn-danger" >mute</button>
                </center>
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        win.document.write(html);
      } else if (title == "not") {
        if (Notification.permission === "granted") {
          new Notification("這是系統通知");
        } else {
          Notification.requestPermission();
        }
      } /*else if (title == "lst") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=500"
        );
        let html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
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
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        win.document.write(html);
      } */ else if (title == "cap") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=500,height=420"
        );
        let html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>截圖提問</h1>
                </center>
                <br>
                <video id="video" autoplay muted playsinline></video>
                <div id="output" style="display: inline"></div> 
                <button type="button" onclick="window.close();" class="btn btn-primary" style="position:absolute;right: 11px;top:100px;">彈跳問題</button>
                <button type="button" onclick="window.close();" class="btn btn-primary" sytle="position:absolute;right: 20px;top:60px;">儲存問題</button>
                <br><br>
                <div class="form-group">
                    <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
                </div>
                <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.3.2/html2canvas.min.js" integrity="sha512-tVYBzEItJit9HXaWTPo8vveXlkK62LbA+wez9IgzjTmFNLMBO1BEYladBw2wnM3YURZSMUyhayPCoLtjGh84NQ==" crossorigin="anonymous" referrerpolicy="no-referrer"></s>
                <script>
                function takeshot() {
                    var video = document.getElementById('video');
                    var w = 16*100;
                    var h = 9*100;
                    var canvas = document.createElement('canvas');
                    canvas.width = w;
                    canvas.height = h;
                    var ctx = canvas.getContext('2d');
                    ctx.drawImage(video, 0, 0, w, h);
                    var img = canvas.toDataURL("image/png");
                    var im = document.createElement('img');
                    im.src=img;
                    im.height=220;
                    return im;
                }
                async function Capture() {
                    try {
                        let videoElem=document.getElementById('video');
                        videoElem.srcObject = await navigator.mediaDevices.getDisplayMedia();
                        var output = document.getElementById('output');
                        
                        setTimeout(()=>{output.appendChild(takeshot());videoElem.height=0},200);

                    } catch(err) {
                        console.error("Error: " + err);
                    }
                }
                function stopCapture() {
                    let tracks = videoElem.srcObject.getTracks();

                    tracks.forEach(track => track.stop());
                    //videoElem.srcObject = null;
                }

                Capture();
                
                



                </s>
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        win.document.write(html);
      } else if (title == "ask") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=200"
        );
        let html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>文字提問</h1>
                </center>
                <div class="form-group">
                    <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
                </div>
                <center>
                <button type="button" onclick="tmp();window.close();" class="btn btn-primary">彈跳問題</button>
                <button type="button" onclick="tmp();window.close();" class="btn btn-primary">儲存問題</button>
                </center>
                <script>
                function tmp(){
                    document.getElementById("exampleFormControlTextarea1").value="";
                }
                </s>
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        win.document.write(html);
      } else if (title == "lst") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=500"
        );
        let html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>問題集</h1>
                </center>
                <ul class="list-group" id="2">
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">CH1 Quiz 2 美國蘋果公司選擇在中國組裝零件為成品，主要是考量下列哪
 個工業區位？</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">CH2 Quiz 1「與荒野相遇，我其實感受的是一種生活，一種與自然澈底相融的生活……每一時的鼻息耳目都浸潤在自
然裡。」請問作者的境界較近似於哪一個選項？</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">CH2 Quiz 2 The roses _____ on the table will be given to Cindy as a birthday gift.
</li>
                    <li id="1" class="list-group-item"><input class="form-check-input" type="checkbox" value="">CH3 Quiz 3 I haven’t been to the movies these days. Are there any good _____ this week? (101 基測)</li>
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
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        win.document.write(html);
      } else if (title == "emg") {
        let win = window.open(
          "",
          "_blank",
          "directories=no,titlebar=no,toolbar=no,location=no,status=no,menubar=no,scrollbars=no,resizable=no,width=400,height=400"
        );
        let html;
        if(this.email.includes('b10111001@zbridge.lwet.sft.tw')){
        html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>緊急</h1>
                </center>
                <h1 class="text-danger">警示語</h1>
                <h3 class="text-danger">注意彈跳時機，<br>老師會被此按鈕打斷，<br>回到視訊頁面。</h3>
                <hr>
                <div class="form-group">
                    <label for="exampleFormControlTextarea1">緊急訊息</label>
                    <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
                </div>
                <br>
                <center>
                <button type="button" onclick="window.close();" class="btn btn-danger">彈跳</button>
                </center>
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        }else{
          html =
          `<head><title>${title}</title><link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous"></head>\
                <body>\
                <center>
                <h1>緊急</h1>
                </center>
                <h1 class="text-danger">警示語</h1>
                <h3 class="text-danger">注意彈跳時機，<br>老師會被此按鈕打斷，<br>回到視訊頁面。</h3>
                <hr>
                <h3 class="text-danger">您尚未取得教師授權，<br>無法進行緊急彈跳。</h3>
                </center>
                </body>`
            .replace("</s>", "</scr" + "ipt>")
            .replace("</s>", "</scr" + "ipt>");
        }
        win.document.write(html);
      }
    },
    addemail() {
      this.email.push(this.ema);
      this.ema = "";
    },
  },
};
</script>
<style scoped>
</style>
