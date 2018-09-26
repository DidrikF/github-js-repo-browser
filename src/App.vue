<template>
  <div id="app">
    <div class="Banner">
        <img class="Banner__logo" src="./assets/GitHub_Logo.png">

        <p>The most popular JavaScript packages on GitHub</p>
    </div>
    <paginated-repository-browser 
        :link='repoBrowserConfig.link'
        :repos-per-page='repoBrowserConfig.reposPerPage'
        :repos-per-request='repoBrowserConfig.reposPerRequest'
        :max-page-number='repoBrowserConfig.maxPageNumber'
        :page-navigation-span='repoBrowserConfig.pageNavigationSpan'
    ></paginated-repository-browser>
  </div>
</template>

<script>
import PaginatedRepositoryBrowser from './components/PaginatedRepositoryBrowser.vue'

export default {
  name: 'App',
  data () {
      return {
          repoBrowserConfig: {
                link: 'https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&per_page=100', // set language to go/php or any other langage, sort like you want etc.
                reposPerPage: 20, // chage how many repos you see per page
                reposPerRequest: 100, // must be changes together with the '$per_page' parameter in the link above
                maxPageNumber: 50, // the developer must know what the maximum page is, with the link above; only the first 1000 repos can be fetched, so max page is 50 with 20 repos per page.
                pageNavigationSpan: 5, // change how many pages users can nivigate to directly, via the nav bar
            }
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
