<template>
  <div id="app">
    <img class="Banner__logo" src="./assets/GitHub-Logos/GitHub_Logo.png">
    <img class="Banner__logo" src="./assets/js_logo.png">
    <span>Repos</span>
    <paginated-repository-browser :link='config.link' :repos-per-page='config.elementsPerPage' :repos-per-requenst='config.reposPerRequest'></paginated-repository-browser>
  </div>
</template>

<script>
import PaginatedRepositoryBrowser from './components/PaginatedRepositoryBrowser.vue'

export default {
  name: 'App',
  data () {
      return {
          config: {
                link: 'https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&per_page=100',
                elementsPerPage: 20,
                reposPerRequest: 100,
          },
      }
  },
  components: {
    PaginatedRepositoryBrowser
  }
}
</script>

<style>

/* Global Styles goes here */

#app {
    width: 800px;
    margin: 0 auto;
}

.Banner__logo {
    max-height: 70px

}

</style>
