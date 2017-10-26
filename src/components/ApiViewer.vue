<template>
  <div class="api-viewer">
    <div class="hero-head container">
      <h1 class="title is-size-1 has-text-centered">CoReA2d</h1>
        <div class="field">
          <div class="control">
            <input class="input" type="text" v-model="search" placeholder="entrer une requête valide ...">
            <p class="help">
              la requête est de la forme suivante : <strong>"couche d'annotation"</strong>-<strong>"identifiant
              de l'entrée"</strong> ex: LST-1
            </p>
          </div>
        </div>
  </div>
  <div class="hero-body">
    <div class="container has-text-centered is-centered" v-if="typeof(entry) == 'object'">
      <div class="notification is-half" v-for="(t, el) in entry" v-if="t && el !== 'size' && el !== 'id'">
        <h4>
          <strong>{{ el }}</strong>
        </h4>
        <div>
          <p>{{ t }}</p>
        </div>
      </div>
    </div>
    <p v-else="" class="container has-text-centered">{{ entry }}</p>
  </div>
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
        dbDict: {
          LST: 'lexique_transdisciplinaire',
          PhrLG: 'lexique_phraseo',
        },
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
          this.entry = 'recherche mal formée ou le champ de recherche vide';
        } else {
          axios.get(`http://localhost:3000/search?db=${this.dbDict[splitSearch[0]]}&id=${splitSearch[1]}`)
            .then((response) => {
              this.entry = response.data;
            })
            .catch(() => {
              this.entry = 'la recherche n\'a pas donnée de résultat ...';
            });
        }
      },
      debounceQuery: _.debounce(function () { this.query(); }, 700),
    },
  };
</script>
