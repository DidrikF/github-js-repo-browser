<template>
  <div id="app">
    <nav>
      <ul>
        <li :key='index' v-for='(pageNumber, index) in currentPagesNavigable'><a>{{ pageNumber }}</a></li>
      </ul>
    </nav>

    <table>
      <tr>
        <th></th>
        <th></th>
        <th></th>
        <th></th>
        <th></th>
      </tr>
      <repository-table-row :key='index' v-for='(repo, index) in currentRepos' :repo='repo'></repository-table-row>
    </table>

  </div>
</template>

<script>
import RepositoryTableRow from './RepositoryTableRow.vue'

import axios from 'axios'

export default {
  name: 'PaginatedRepositoryBrowser',
  data () {
    return {
      currentPage: 1,
      currentSubPage: 1,
      repos: [],
      totalNumberOfRepos: 0,
    }
  },
  props: ['link', 'reposPerRequenst','reposPerPage'],
  computed: {
    totalSubPages () {
      return this.totalNumberOfRepos / this.reposPerPage
    },
    totalPages () {
      this.totalNumberOfRepos / this.reposPerRequenst
    },
    currentPagesNavigable () {
      // based on this.currentPage relative to max and min page
    },
    currentRepos () {
      return this.repos.slice(0,20);
    }
  },
  created () {
    axios({
        method: 'get',
        url: this.link
      }).then(response => {
        this.repos = response.data.items
        this.totalNumberOfRepos = response.data.total_count
      }).catch(err => {
        console.log(err)
      })
  },
  methods: {
    changePage (subPage) {
      // what requestPage does this correspond to? If we dont have it, get it!
    }
  },
  components: {
    RepositoryTableRow
  }
}
</script>

<style>




</style>
