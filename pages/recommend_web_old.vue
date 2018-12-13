
<template>
  <section>
    <!-- <div>
     //デバッグゾーン
      {{q_num}}
      {{visitor_tags}}
    </div> -->

    <div v-if="q_num==0" class="title">
      <h3><p>web推薦のページです<br><br>
      表示される語句の中から<br>気になるものを<br>
      選んでください<br>(複数回答可)</p></h3>
      <v-spacer><br></v-spacer>
    </div>

    <!-- 質問の進捗表示 -->
    <div v-if="q_num>0&&q_num<=data_query.length">
      {{q_num}}/{{data_query.length}}
    </div>

    <!-- 質問表示 -->
    <div
    class="query"
    v-for="(item,index) in data_query"
    :key="index">
      <div
      v-if="q_num==index+1">
        <div v-for="item_a in item"
        :key="item_a.tag_id">
          <v-checkbox
          height = 2
          v-model="selected"
          :label="item_a.tag"
          :value="[item_a.tag,item_a.tag_id]"></v-checkbox>
        </div>
      </div>
    </div>


    <!-- 推薦展示物表示 -->
    <div
    v-if="q_num==-10000">
      <v-layout column width="350px"
      v-for="(item,index) in recommend_exhibits"
      :key="index">
        <v-flex>
          <v-spacer><br></v-spacer>
          <v-card light raised>
            <v-chip color="orange" text-color="white" class="rank">
              オススメ{{index+1}}位
              <v-icon right>star</v-icon>
            </v-chip>

            <v-img
              class="card_img"
              :src="item.src"
              width="350px"
            >
            </v-img>

            <v-card-title primary-title class="primary-title">
              <div>
              <div class="headline">{{item.exhibit}}</div>
              <v-spacer><br></v-spacer>

              <div class="basis">
                <span
                v-for="tag in item.basis"
                :key="tag">
                  <v-chip
                  class="basis"
                  color="secondary"
                  text-color="white"
                  disable
                  small>
                    <v-icon>label</v-icon>{{ tag }}
                  </v-chip>
                </span>
              </div>
              </div>
            </v-card-title>

            <v-card-actions class="actions">
              <v-btn
              flat
              outline
              color="purple"
              :href="item.url"
              target="_blank"
              >解説を見る</v-btn>
              <v-btn flat outline color="accent" @click="show[index] = !show[index]">
                概要
                <v-icon>{{ show[index] ? 'keyboard_arrow_up' : 'keyboard_arrow_down' }}</v-icon>
              </v-btn>
            </v-card-actions>

            <v-slide-y-transition>
              <v-img class="more" v-show="show[index]"
                :src="item.map"
                width="350px">
              </v-img>
            </v-slide-y-transition>
          </v-card>
          <v-spacer><br></v-spacer>
        </v-flex>
      </v-layout>
    </div>

    <!-- テストボタン -->
    <!-- <div>
      <v-btn v-if="q_num==0" round color="primary" large v-on:click="show_result">テスト</v-btn>
    </div> -->

    <!-- スタートボタン -->
    <div>
      <v-btn v-if="q_num==0" round color="primary" large v-on:click="web_reccomend">スタート</v-btn>
    </div>

    <!-- 初期ロード表示 -->
    <div class="loading">
      <v-progress-circular
      v-if="q_num==-5000"
      :size="50"
      color="primary"
      indeterminate
    ></v-progress-circular>
    </div>

    <div>
      <v-btn v-if="q_num!=0&&data_query&&0<=q_num&&q_num<=data_query.length" round color="primary" large v-on:click="get_checkbox">次へ</v-btn>
    </div>

    <div v-if="q_num!=0&&data_query&&q_num>data_query.length">
      <p>お疲れ様でした！</p>
      <v-btn round color="primary" large v-on:click="show_result">結果を表示する</v-btn>
    </div>

    <!-- ロード中 -->
    <div v-if="q_num==-9000">
      <v-progress-circular
      :buffer-value="10"
      :rotate="-90"
      :size="200"
      :width="15"
      :value="value"
      color="primary"
    >
        分析中...
      </v-progress-circular>
    </div>

  </section>
</template>




<script>
import AppLogo from '~/components/AppLogo.vue'

export default {
  layout: 'nested',
  components: {
    AppLogo
  },
  data(){
    return{
      interval: {}, //progress
      value: 0, //progress
      show: {0: false, 1: false, 2: false},
      json: null,
      result: null,
      q_num: -5000,
      tags: [
        {tag:'宇宙像',tag_id:1},
        {tag:'素粒子',tag_id: 2},
        {tag:'スケール',tag_id: 3},
        {tag:'大きな数',tag_id: 4},
        {tag:'地球からの距離',tag_id: 5},
        {tag:'地球',tag_id: 6},
        {tag:'惑星',tag_id: 7},
        {tag:'大陸移動',tag_id: 8},
        {tag:'プレート',tag_id: 9},
        {tag:'自転',tag_id: 10},
        {tag:'月食',tag_id: 11},
        {tag:'月の満ち欠け',tag_id: 12},
        {tag:'衛星',tag_id: 13},
        {tag:'月齢',tag_id: 14},
        {tag:'かぐや',tag_id: 15},
        {tag:'宇宙開発競争',tag_id: 16},
        {tag:'月面探査',tag_id: 17},
        {tag:'有人飛行',tag_id: 18},
        {tag:'ロケット',tag_id: 19},
        {tag:'巨大模型',tag_id: 20},
        {tag:'ボイジャー1号',tag_id: 21},
        {tag:'流星群',tag_id: 22},
        {tag:'隕石',tag_id: 23},
        {tag:'オーロラ',tag_id: 24},
        {tag:'彗星',tag_id: 25},
        {tag:'万有引力',tag_id: 26},
        {tag:'ニュートン',tag_id: 27},
        {tag:'ケプラー',tag_id: 28},
        {tag:'ブラックホール',tag_id: 29},
        {tag:'物理法則',tag_id: 30},
        {tag:'惑星探査',tag_id: 31},
        {tag:'ミッション',tag_id: 32},
        {tag:'人工衛星',tag_id: 33},
        {tag:'実体験展示',tag_id: 34},
        {tag:'プロジェクト',tag_id: 35},
        {tag:'地球外生命体',tag_id: 36},
        {tag:'系外惑星',tag_id: 37},
        {tag:'星の明るさ',tag_id: 38},
        {tag:'星までの距離',tag_id: 39},
        {tag:'星の大きさ',tag_id: 40},
        {tag:'星の位置関係',tag_id: 41},
        {tag:'北斗七星',tag_id: 42},
        {tag:'カシオペア座',tag_id: 43},
        {tag:'オリオン座',tag_id: 44},
        {tag:'夏の大三角',tag_id: 45},
        {tag:'核融合',tag_id: 46},
        {tag:'星雲',tag_id: 47},
        {tag:'スペクトル',tag_id: 49},
        {tag:'超新星',tag_id: 50},
        {tag:'銀河群',tag_id: 51},
        {tag:'アンドロメダ',tag_id: 52},
        {tag:'銀河',tag_id: 53},
        {tag:'銀河団',tag_id: 54},
        {tag:'銀河系',tag_id: 56},
        {tag:'天の川',tag_id: 57},
        {tag:'円盤',tag_id: 58},
        {tag:'アクリル模型',tag_id: 59},
        {tag:'銀河の種類',tag_id: 60},
        {tag:'宇宙の果て',tag_id: 61},
        {tag:'ビッグバン',tag_id: 62},
        {tag:'宇宙の地図',tag_id: 63},
        {tag:'インフレーション',tag_id: 64},
        {tag:'宇宙の大きさ',tag_id: 65}],
      data_query: null, // , {tag:'ブラックホール',tag_id: 48}, {tag:'ブラックホール',tag_id: 55}
      visitor_tags: [],
      // visitor_tags_test: [['万有引力',26],['ケプラー',28] , ['ブラックホール', 29], ['ニュートン', 27], ['物理法則',30]],
      selected: [],
      recommend_exhibits: []
    }
  },
  methods: {
    show_result: function(event){

      this.q_num = -9000 //値は適当です
      const recommend_num = 3; //オススメする展示物の数
      const adopted_num = 2; //pmi上位採用数
      let a516 = {id: 516,exhibit: "パワーズオブテン",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/42",point: 0, pmi: 0,basis: [],src: require("../static/A516-photo.jpg"),map: require("../static/background.png")}
      let a517 = {id: 517,exhibit: "地球",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/43",point: 0,pmi: 0,basis: [],src: require("../static/A517-photo.jpg"),map: require("../static/background.png")}
      let a518 = {id: 518,exhibit: "月の満ち欠け",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/44",point: 0,pmi: 0,basis: [],src: require("../static/A518-photo.jpg"),map: require("../static/background.png")}
      let a519 = {id: 519,exhibit: "月への挑戦",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/45",point: 0,pmi: 0,basis: [],src: require("../static/A519-photo.jpg"),map: require("../static/background.png")}
      let a520 = {id: 520,exhibit: "太陽系",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/46",point: 0,pmi: 0,basis: [],src: require("../static/A520-photo.jpg"),map: require("../static/background.png")}
      let a521 = {id: 521,exhibit: "惑星の動きと引力",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/47",point: 0,pmi: 0,basis: [],src: require("../static/A521-photo.jpg"),map: require("../static/background.png")}
      let a522 = {id: 522,exhibit: "惑星探査",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/48",point: 0,pmi: 0,basis: [],src: require("../static/A522-photo.jpg"),map: require("../static/background.png")}
      let a523 = {id: 523,exhibit: "宇宙を測る・宇宙を探る",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/49",point: 0,pmi: 0,basis: [],src: require("../static/A523-photo.jpg"),map: require("../static/background.png")}
      let a524 = {id: 524,exhibit: "星座を形づくる星々",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/50",point: 0,pmi: 0,basis: [],src: require("../static/A524-photo.jpg"),map: require("../static/background.png")}
      let a525 = {id: 525,exhibit: "星の世界",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/51",point: 0,pmi: 0,basis: [],src: require("../static/A525-photo.jpg"),map: require("../static/background.png")}
      let a526 = {id: 526,exhibit: "銀河の世界",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/52",point: 0,pmi: 0,basis: [],src: require("../static/A526-photo.jpg"),map: require("../static/background.png")}
      let a527 = {id: 527,exhibit: "銀河系と天の川",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/53",point: 0,pmi: 0,basis: [],src: require("../static/A527-photo.jpg"),map: require("../static/background.png")}
      let a528 = {id: 528,exhibit: "宇宙の果て",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/54",point: 0,pmi: 0,basis: [],src: require("../static/A528-photo.jpg"),map: require("../static/background.png")}
      let exhibits_list = [a516,a517,a518,a519,a520,a521,a522,a523,a524,a525,a526,a527,a528]
      let visitor_tags = this.visitor_tags
      let db = [];


      //分析中...表示用
      this.interval = setInterval(() => {
        if (this.value === 110) {
          // return (this.value = 0)
          this.q_num = -10000
        }
        this.value += 5
      }, 200)




      for(let v of visitor_tags){
        let tmp_json = [];
        let tmp = this.json.filter(function(element){
          return element.visitor_tag_id == v[1]
        })
        // console.log(tmp)
        for (let w of tmp){
          let tmp1 = {}
          tmp1.exhibit = w.exhibit
          tmp1.exhibit_id = w.exhibit_id
          tmp1.exhibit_tag = w.exhibit_tag
          tmp1.visitor_tag = w.visitor_tag
          tmp1.visitor_tag_id = w.visitor_tag_id
          tmp1.pmi = w.pmi_diff_from_med
          tmp_json.push(tmp1)
        }
        // console.log(tmp_json)
        db.push(tmp_json)
      }
      // console.log(db)
      let db_unique = []
      for(let d of db){
        var arrObj = {};
        for (var n = 0; n < d.length; n++) {
          arrObj[d[n]['exhibit_tag']] = d[n];
        }
        d = [];
        for (var key in arrObj) {
          d.push(arrObj[key]);
        }
        db_unique.push(d)
      }

      // console.log(db)
      let db_sorted = [];
      for(let v of db_unique){
        v.sort((a, b) => {
        if (a.pmi > b.pmi) return -1;
        if (a.pmi < b.pmi) return 1;
        return 0;
        });
        db_sorted.push(v)
      }
      // console.log(db_sorted)
      for(let v of db_sorted){ // v = 65行
        // console.log(v)
        for(let i=0; i<adopted_num; i++){
          let tmp_exhibit_tag = v[i].exhibit_tag
          let tmp_exhibit_id = v[i].exhibit_id
          let tmp_json = this.json.filter(function(element){
            return element.exhibit_tag == tmp_exhibit_tag
          })
          // console.log(tmp_json)

          // tmp_jsonの重複除去
          var arrObj = {};
          for (var n = 0; n < tmp_json.length; n++) {
            arrObj[tmp_json[n]['exhibit']] = tmp_json[n];
          }
          tmp_json = [];
          for (var key in arrObj) {
            tmp_json.push(arrObj[key]);
          }

          // console.log(tmp_json)
          for(let l of tmp_json){
            for(let w of exhibits_list){
              if(w.id===l.exhibit_id){
                w.point++;
                w.pmi=w.pmi+(Math.round(v[i].pmi*100))/100;
                w.basis.push(v[i].visitor_tag)
                // console.log(exhibits_list)
              }
            }
          }
        }
      }

      exhibits_list.sort(function(a, b){
	       if (a.point < b.point) return 1;
	       if (a.point > b.point) return -1;
         if (a.pmi < b.pmi) return 1;
	       if (a.pmi > b.pmi) return -1;
	       return 0;
      });

      // console.log(exhibits_list)
      // for(let i=0; i<recommend_num; i++){
      //   var arrObj = {};
      //   for (var n = 0; n < exhibits_list[i].length; n++) {
      //     arrObj[tmp_json[n]['basis']] = tmp_json[n];
      //   }
      //   tmp_json = [];
      //   for (var key in arrObj) {
      //     tmp_json.push(arrObj[key]);
      //   }
      // }


      for(let i=0; i<recommend_num; i++){
      //   var overlap = exhibits_list[i].basis.filter(function (x, i, self) {
      //       return self.indexOf(x) !== self.lastIndexOf(x);
      //   });
        // console.log(exhibits_list[i].basis)
        var unique = exhibits_list[i].basis.filter(function (x, i, self) {
            return self.indexOf(x) === i;
        });


        // x3 とかの表示
        // console.log(overlap)
        // console.log(unique)
        //
        // var counts = {};
        //
        // for(var k=0;k< overlap.length;k++)
        // {
        //   var key = overlap[k];
        //   counts[key] = (counts[key])? counts[key] + 1 : 1 ;
        // }

        // for(let v in counts){
        //   for(let j=0; j<unique.length; j++){
        //     if(v===unique[j]){
        //       unique[j] = unique[j] + "x" + counts[v]
        //     }
        //   }
        // }

        exhibits_list[i].basis = unique
        // console.log(exhibits_list)
        this.recommend_exhibits.push(exhibits_list[i])
      }
      // console.log(this.recommend_exhibits)

    },

    get_checkbox: function(event){
      // this.visitor_tags.push(this.selected[0])
      for(let i=0; i<this.selected.length; i++){
        this.visitor_tags.push(this.selected[i])
      }
      this.q_num++;
      this.selected.length = 0;
    },
    web_reccomend: function (event) {
      // this.$router.push({ path: 'hoge', query: { plan: 'pri', aaa: 'AAA'}})
      // console.log(this.json)
      // const tmp = this.json.find(function(element){
      //   return element.id == 3078
      // })
      // const tmp = this.json.filter(function(element){
      //   return element.visitor_tag_id == 30
      // })
      // console.log(tmp)
      // this.result = tmp[0].id
      // this.isResultShow = true
      // ここにオススメプログラムをつらつらと書いていく
      // let tags;
      // tags = this.json.filter(function(element){
      //   return element.visitor_tag_id == 2
      // })
      let array = this.tags;
      let tags; //来館者タグ一覧
      const q_tag_num = 7 //一回あたりのタグ提示数
      // let tags
      let q_times = []
      let query = []

      for(let i = array.length - 1; i > 0; i--){
        let r = Math.floor(Math.random() * (i + 1));
        let tmp = array[i];
        array[i] = array[r];
        array[r] = tmp;
        tags = array
      }
      // 質問クエリの単語数を決める
      let tmp = tags.length%q_tag_num
      if (tmp <= Math.ceil(q_tag_num/2)){
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num+1);
        }
        tmp = (tags.length-q_times.length*(q_tag_num+1))/q_tag_num
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num);
        }
      }else{
        for (let i=0; i<q_tag_num-tmp; i++){
          q_times.push(q_tag_num-1);
        }
        tmp = (tags.length-q_times.length*(q_tag_num-1))/q_tag_num
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num);
        }
      } // q_times = [8, 8, 7, 7, 7, 7, 7, 7, 7]

      // 質問クエリを作成する
      for (let i=0; i<q_times.length; i++){
        query.push(tags.slice(0,q_times[i]))
        tags.splice(0,q_times[i])
      }

      this.data_query = query;

      this.q_num++;

      // console.log(this.q_num)

    }
  },



  created() {
    //do something after creating vue instance
    // console.log('こんいgんkg')
    this.json = require('../static/mysql.json')
    // this.q_num = 0;
  },

  mounted() {
    //do something before mounting vue instance
    this.q_num = 0;
  }
  // asyncData(){
  //   // console.log('testss')
  //   return{
  //     json:require('../static/mysql.json')
  //   }
  // }
}
</script>

<style>
/* .basis{
  width: 350px;
} */
.query{
  font-weight: bold;
  font-size: 2em;
}
.title{
  font-size: 14px;
  font-weight: bold;
}
.card_img{
  justify-content: center;
  align-items: center;
}
.actions{
  justify-content: center;
}
.primary-title{
  align-items: center;
  justify-content: center;
  width: 350px;
}
.more {
  max-width: 350px;
}

.rank {
  font-size: 16px;
  font-weight: bold;
}
.container {
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}

</style>
