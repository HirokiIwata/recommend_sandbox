<template>
  <v-responsive>
    <v-container fill-height>
      <v-layout class="main">
        <v-flex>
          <h4 class="display-3">Welcome to <br>the site</h4>

          <span class="subheading">天文展示室の展示物をオススメします！</span>

          <v-divider class="my-3"></v-divider>

          <div class="title mb-3">↓↓まずはここをチェック！↓↓</div>

          <router-link to="/recommend_web">
            <v-btn
              class="mx-0"
              color="primary"
              large
            >
              展示物のオススメへ行く
            </v-btn>
          </router-link>
        </v-flex>
      </v-layout>
    </v-container>
  </v-responsive>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'

export default {
  layout: 'nested',
  components: {
    AppLogo
  },
  methods: {
    junbichu: function (event) {
      // ここにオススメプログラムをつらつらと書いていく
      alert('準備中です！')
    }
  }
}
</script>

<style>
.main{
  align-items: center;
  justify-content: center;
}
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
