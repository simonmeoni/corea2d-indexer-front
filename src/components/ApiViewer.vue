<template>
  <div class="api-viewer">
    <div class="hero-head container">
      <h1 class="is-size-1 has-text-centered">CoReA2D</h1>
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
      <div class="notification is-half" v-for="(t, el) in entry" v-if="t">
        <h4>
          <strong>{{ labelDict[el] ? labelDict[el] : el[0].toUpperCase() + el.slice(1)}}</strong>
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
          lst: 'lexique_transdisciplinaire',
          ls: 'lexique_transdisciplinaire',
          l: 'lexique_transdisciplinaire',
          PhrLG: 'lexique_phraseo',
          ph: 'lexique_phraseo',
          phrlg: 'lexique_phraseo',
          lg: 'lexique_phraseo',
        },
        labelDict: {
          class: 'Classe Sémantique',
          definition: 'Définition',
          pos: 'Part-Of-Speech',
          super_entry: 'Entrée Sémantique',
          norm_form: 'Forme Normalisée',
          tlfi_norm: 'Forme Extraite du TLFi',
          lexical_entry: 'Entrée Lexicale',
          normalized_form: 'Domaine concerné',
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
