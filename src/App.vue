<template>
  <div id="app">
    <div class="Banner">
        <img class="Banner__logo" src="./assets/GitHub_Logo.png">

        <p>The most popular JavaScript packages on GitHub</p>
    </div>
    <paginated-repository-browser :link='link'></paginated-repository-browser>
  </div>
</template>

<script>
import PaginatedRepositoryBrowser from './components/PaginatedRepositoryBrowser.vue'

export default {
  name: 'App',
  data () {
      return {
            link: 'https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&per_page=100',
        }
  },
  components: {
    PaginatedRepositoryBrowser
  }
}
</script>

<style>

#app {
    width: 800px;
    margin: 0 auto;
}

.Banner {
    text-align: center;
}

.Banner__logo {
    max-height: 70px;
}

</style>
