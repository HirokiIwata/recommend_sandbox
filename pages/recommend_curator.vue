
<template>
  <section>
    <div v-if="q_num==0">
      <p>学芸員推薦のページです</p>
    </div>
    <div>
      <v-btn v-if="q_num==0" round color="primary" large v-on:click="curator_reccomend">スタート</v-btn>
    </div>

    <div v-if="q_num==-9000">
      <v-progress-circular
      :buffer-value="10"
      :rotate="-90"
      :size="200"
      :width="15"
      :value="value"
      color="primary"
    >
        準備中...
      </v-progress-circular>
    </div>

    <div
    v-if="q_num==-10000">
      <v-layout column width="350px"
      v-for="(item,index) in recommend_exhibits"
      :key="index">
      <v-flex>
        <v-spacer><br></v-spacer>
        <v-card light raised>
          <v-chip color="orange" text-color="white" class="rank">
            オススメ
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

            <div>
              ここにオススメの理由を表示する
            </div>
            </div>
          </v-card-title>

          <v-card-actions class="actions">
            <v-btn flat outline color="accent" @click="show = !show">
              概要
              <v-icon>{{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}</v-icon>
            </v-btn>
            <v-btn flat outline color="purple">解説を見る</v-btn>
            <v-btn flat outline color="primary">アクセス</v-btn>
          </v-card-actions>

          <v-slide-y-transition>
            <v-card-text class="more" v-show="show">
              {{item.overview}}
            </v-card-text>
          </v-slide-y-transition>
        </v-card>
        <v-spacer><br></v-spacer>
      </v-flex>
    </v-layout>
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
      q_num: -5000,
      recommend_exhibits: [],
      show: false,
      value: 0,
      interval: {}
    }
  },
  methods: {
    curator_reccomend: function (event) {
      // ここにオススメプログラムをつらつらと書いていく
      this.q_num = -9000
      let recommend_exhibits = [
        {
          id: 516,
          exhibit: "パワーズオブテン",
          src: require("../static/A516-photo.jpg"),
          overview: "この展示室の外周は北側入口から右回りに、地球から宇宙の果てまでの天体や事象を並べています。そのスケールは、長さを10倍ずつ大きくしていくパワーズオブテン（10のべき乗）の考え方に沿っています。 北側展示室入り口から時計回りに外周を回って、宇宙のスケールの旅にご出発ください。"
        }
      ]
      for(let v of recommend_exhibits){
        this.recommend_exhibits.push(v)
      }

      //分析中...表示用
      this.interval = setInterval(() => {
        if (this.value === 110) {
          // return (this.value = 0)
          this.q_num = -10000
        }
        this.value += 5
      }, 200)
    }
  },
  mounted() {
    //do something before mounting vue instance
    this.q_num = 0;
  }
}
</script>

<style>
card_img{
  justify-content: center;
}
.actions{
  justify-content: center;
}
.primary-title{
  align-items: center;
  justify-content: center;
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
