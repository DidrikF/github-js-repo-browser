<template>
  <tr>
    <td>
      <img class="Avatar" v-bind:src="repo.owner.avatar_url" alt="Owner Avatar">
    </td>
    <td>
      <div>
        {{ repo.name }}
      </div>
    </td>
    <td></td>
    <td></td>
  </tr>
  <!-- 
    INTERESTING REPO INFORMATION

    - owner.avarat_url

    Repo info:
    - name / full_name
    Owner info
      - owner.avarat_url
      - owner.login
      - owner.type
      - owner.html_url // url to 
      - created_at
    

    - description

    Stats:
    - score // ranking
    - stargazers_count // number of stars
    - watchers
    - forks
    - open_issues
    - size (what does this mean?)


    NOTES
    get a js logo
  -->
</template>

<script>


export default {
  name: 'RepositoryTableRow',
  data () {
    return {
      
    }
  },
  props: ['repo'],
  methods: {

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.Avatar {
  width: 50px;
  height: 50px;
  border-radius: 5px;
}

</style>
