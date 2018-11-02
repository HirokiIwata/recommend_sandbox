<template>
  <v-jumbotron>
    <v-container fill-height>
      <v-layout align-center>
        <v-flex>
          <h3 class="display-3">Welcome to the site</h3>

          <span class="subheading">名古屋市科学館<br>天文展示室の展示物をオススメします！</span>

          <v-divider class="my-3"></v-divider>

          <div class="title mb-3">Check out our newest features!</div>

          <v-btn
            class="mx-0"
            color="primary"
            large
          >
            See more
          </v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </v-jumbotron>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'

export default {
  layout: 'nested',
  components: {
    AppLogo
  }
}
</script>

<style>

</style>
