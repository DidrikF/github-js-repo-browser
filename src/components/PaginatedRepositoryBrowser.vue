<template>
  <div id="app">

    <nav class="PageNavigation">
      <ul class="PageNavigation__List">
        <li class="PageNavigation__Element" :key='index' v-for='(pageNumber, index) in currentPagesNavigable'>
           
          <span v-if='(currentPagesNavigable[1] > 2) && (currentPagesNavigable[index-1] === 1)'>...</span>
          <span v-if='(currentPagesNavigable[currentPagesNavigable.length-2] < currentPagesNavigable[currentPagesNavigable.length-1] - 1) && (currentPagesNavigable[currentPagesNavigable.length-1] === pageNumber)'>...</span>
          
          <a href="#" v-if='pageNumber === currentPage' class='PageNavigation__Element--active' @click='changePage(pageNumber)'>{{ pageNumber }}</a>
          <a href="#" v-else @click='changePage(pageNumber)'>{{ pageNumber }}</a>
          
        </li>
      </ul>
    </nav>

    <table class="RepoTable">
      <repository-table-row :key='index' v-for='(repo, index) in currentlyDislayedRepos' :repo='repo' :rank='firstRepoOnPage + index + 1'></repository-table-row>
    </table>

  </div>
</template>

<script>
import RepositoryTableRow from './RepositoryTableRow.vue'

import axios from 'axios'
import { range } from 'lodash'

export default {
  name: 'PaginatedRepositoryBrowser',
  data () {
    return {
      currentPage: 1,
      repos: [],
      firstRepoOnPage: 1,
      currentGitHubPageLoaded: 1,
      githubLink: '',
    }
  },
  props: ['link', 'pageNavigationSpan', 'maxPageNumber', 'reposPerRequest', 'reposPerPage'],
  computed: {
    /**
     * Computed property returning an array of the page numbers that are currently navigable. That is; the current page pluss/minus pageNavigationSpan, the first and last page.
     * Only the first 1000 repositories from 'https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&per_page=100&page=X'
     * are avialable for consumption. This is why 50 is hard-coded as the highest page number
     */
    currentPagesNavigable () {
      const navigablePages = range(this.currentPage-this.pageNavigationSpan, this.currentPage+this.pageNavigationSpan+1);
      if (this.currentPage-this.pageNavigationSpan > 1) {
        navigablePages.unshift(1)
      }
      if (this.currentPage+this.pageNavigationSpan+1 < this.maxPageNumber) {
        navigablePages.push(this.maxPageNumber)
      }
      return navigablePages.filter(page => (page >= 1) && (page <= this.maxPageNumber)) 

    },
    /**
     * Computed property returrning an array of the 20 elements that corresponds to the current page.
     */
    currentlyDislayedRepos () {
      /* Used as a reference point for the rank prop passed down to the RepositoryTableRow components. */
      this.firstRepoOnPage = (this.currentPage-1)*this.reposPerPage

      const firstRepo = Math.floor(((this.currentPage-1)*this.reposPerPage) % this.reposPerRequest)
      const lastRepo = firstRepo + this.reposPerPage

      return this.repos.slice(firstRepo, lastRepo)
    }
  },
  methods: {
    /**
     * Calculates what page of 100 elements from GitHub that corresponds to the page in the app of 20 elements.
     * If this is not the page currently loaded from GitHub, the correct page is loaded and the state of the
     * component is updated. 
     */
    changePage (page) {
      const pageToGetFromGithub = Math.ceil(page / (this.reposPerRequest/this.reposPerPage))

      if (pageToGetFromGithub !== this.currentGitHubPageLoaded) {
        axios({
          method: 'get',
          url: this.githubLink + pageToGetFromGithub
        }).then(response => {
          this.repos = response.data.items
          this.currentGitHubPageLoaded = pageToGetFromGithub
          this.currentPage = page

        }).catch(err => {
          console.log(err)
        })
      } else {
        this.currentPage = page;
      }
    },
  },
  /**
   * Loads the first page of repositories from github and sets the initial state, if successfull.
   * Due to GitHub returning the appropriate links to navigate to the next, previous, first and last pages,
   * this link is extracted from the Link header of the http response. This link can now be used to get any
   * desired page.
   */
  created () {
    axios({
        method: 'get',
        url: this.link
      }).then(response => {
        console.log(response)
        this.repos = response.data.items
        this.currentGitHubPageLoaded = 1
        this.githubLink = response.headers.link.split(/[<>]/)[1].match(/(https:\/\/api\.github\.com.+&page=)/)[0]

      }).catch(err => {
        console.log(err)
      })
  },
  components: {
    RepositoryTableRow
  }
}
</script>

<style scoped>

.PageNavigation {
  text-align: center;
}

.PageNavigation__List {
  padding: 0;
}

.PageNavigation__List>li {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.PageNavigation__Element {
  display: inline-block;
}

.PageNavigation__Element>a {
  display: inline-block;
  color: black;
  text-align: center;
  text-decoration: none;
  padding: 0;
  margin-left: 20px;
}

.PageNavigation__Element>span {
  margin-left: 20px;
}

.PageNavigation__Element>.PageNavigation__Element--active {
  text-decoration: underline;
  color:  #8acc97;
}

.RepoTable {
  border-collapse: collapse;
  width: 100%;
}

.RepoTable>tr:first-child {
  border-radius: 10px 10px 0 0;
}

.RepoTable>tr:last-child {
  border-radius: 0 0 10px 10px;
}


</style>
