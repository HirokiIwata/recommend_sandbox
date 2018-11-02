
<template>
  <section>
    <div>
      <p>学芸員推薦のページです</p>
    </div>
    <div>
      <v-btn round color="primary" large v-on:click="curator_reccomend">スタート</v-btn>
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
  methods: {
    curator_reccomend: function (event) {
      // ここにオススメプログラムをつらつらと書いていく
      alert('準備中です！')
    }
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
