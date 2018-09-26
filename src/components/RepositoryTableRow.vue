<template>
  <tr class="RepoRow" @click="redirect(repo.owner.html_url)">
    <td class="RepoRow__Rank">
      <span>{{ rank }}</span>
    </td>
    <td class="RepoRow__Avatar">
      <img class="Avatar" v-bind:src="repo.owner.avatar_url" alt="Owner Avatar">
    </td>
    <td class="RepoRow__Name">
      <div>
        {{ repo.name }}
      </div>
    </td>
    <td></td>
    <td class="RepoRow__Stats">
      <table>
        <tr>
          <td><i class="fas fa-star"></i></td>
          <td>{{ repo.stargazers_count }}</td>
          <td><i class="fas fa-code-branch"></i></td>
          <td>{{ repo.forks }}</td>
          <td><i class="fas fa-trophy"></i></td>
          <td>{{ repo.score }}</td>
        </tr>
        <tr>
          <td><i class="fas fa-exclamation-circle"></i></td>
          <td>{{ repo.open_issues }}</td>
          <td><i class="fas fa-weight"></i></td>
          <td>{{ repo.size }}</td>
        </tr>
        <tr>
          <td><i class="far fa-calendar-alt"></i></td>
          <td colspan='5'>{{ repo.created_at }}</td>  
        </tr>
      </table>
    </td>
  </tr>
</template>

<script>
import dataFormat from 'dateformat'

export default {
  name: 'RepositoryTableRow',
  props: ['repo', 'rank'],
  methods: {
    /**
     * Redirect the user to the given URL.
     * @param {string} url
     */
    redirect (url) {
      window.location.replace(url)
    },
  },
}
</script>


<style scoped>

.Avatar {
  height: 100px;
  border-radius: 5px;
}

.RepoRow>td:first-child {
  width: 100px;
  text-align: center;
}

.RepoRow {
  height: 100px;
  width: 100%;
  margin: 0;
  padding: 10px;
  border: 1px solid #65758e;
  display: block;
  border-color: #65758e;
  cursor: pointer;
}

.RepoRow:hover {
  background-color: #a6c8fc;
}

.RepoRow__Rank {
  font-size: 2em;
}

.RepoRow__Name {
  padding-left: 30px;
  font-size: 1.5em;
  width: 300px;
}

</style>
