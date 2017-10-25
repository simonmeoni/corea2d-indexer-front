<template>
  <div class="api-viewer">
    <input v-model="search" placeholder="entrer une requête valide ...">
    <ul v-if="typeof(entry)== 'object'"><li v-for="(t, el) in entry">{{ el }} : {{ t }}</li></ul>
    <p v-else>{{ entry }}</p>
  </div>
</template>

<script>
  import axios from 'axios';
  import _ from 'lodash';

  export default {
    name: 'ApiViewer',
    data() {
      return {
        entry: '',
        search: '',
      };
    },
    watch: {
      search() {
        this.entry = '...';
        this.debounceQuery();
      },
    },
    methods: {
      query() {
        const splitSearch = this.search.split('-');
        if (splitSearch.length !== 2 || splitSearch[1] === '') {
          this.entry = 'recherche mal formée ou champ de recherche vide';
        } else {
          axios.get(`http://localhost:3000/search?db=${splitSearch[0]}&id=${splitSearch[1]}`)
            .then((response) => {
              this.entry = response.data;
            })
            .catch(() => {
              this.entry = 'la recherche n\'a pas donnée de résultat ...';
            });
        }
      },
      debounceQuery: _.debounce(function () { this.query(); }, 300),
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
