<script>
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return {
      query: '', // testo inserito nella search bar
      movies: [], // array per memorizzare i film trovati
      seriesTv: [], // array per memorizzare le serie tv trovate
    };
  },
  components: {
    AppHeader,
  },
  methods: {
    // Metodo per cercare sia i film che le serie TV tramite l'API
    searchAll() {
      // Se l'input è vuoto, non effettuiamo la richiesta
      if (this.query.trim() === '') {
        return;
      }

      // Chiamata API per cercare i film
      this.searchMovies();

      // Chiamata API per cercare le serie TV
      this.searchSeries();
    },

    // Metodo per cercare i film tramite l'API
    searchMovies() {
      const apiUrl = `https://api.themoviedb.org/3/search/movie`;

      // Chiamata API con params
      axios
        .get(apiUrl, {
          params: {
            api_key: '6a05e22584f00aace07e6671b782a12d', // inserisco la mia chiave API
            query: this.query, // il testo di ricerca inserito dall'utente
          },
        })
        .then((response) => {
          // La risposta contiene i risultati nella chiave 'results'
          this.movies = response.data.results;
        })
        .catch((error) => {
          console.error('Errore durante la chiamata API per i film:', error);
        });
    },

    // Metodo per cercare le serie TV tramite l'API
    searchSeries() {
      const apiUrl = `https://api.themoviedb.org/3/search/tv`;

      // Chiamata API con params
      axios
        .get(apiUrl, {
          params: {
            api_key: '6a05e22584f00aace07e6671b782a12d', // inserisco la mia chiave API
            query: this.query, // il testo di ricerca inserito dall'utente
          },
        })
        .then((response) => {
          // La risposta contiene i risultati nella chiave 'results'
          this.seriesTv = response.data.results;
        })
        .catch((error) => {
          console.error('Errore durante la chiamata API per le serie TV:', error);
        });
    },
    //metodo per richiamare immagini al posto della scritta "lingua originale" se non è in en-it-ja mette un immagine generale 
    getFlags(lang){
      const validlangs =[
        'en',
        'it',
        'ja'
      ];
      if (validlangs.includes(lang)){
        if (lang == 'en'){
          return '/img/flags/uk-flag.gif';
        }
        else if (lang == 'it'){
          return '/img/flags/it-flag.gif';
        }
        else if (lang == 'ja'){
          return '/img/flags/ja-flag.gif';
        }
      }else {
        return '/img/flags/Flag_with_question_mark.svg.png';
      }
    }
  },
};
</script>

<template>
  <div>
    <!-- Header -->
    <AppHeader />

    <!-- Search Bar e Button -->
    <form class="form-inline my-2 my-lg-0" @submit.prevent="searchAll">
      <input v-model="query" class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" />
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">
        Search
      </button>
    </form>

    <!-- Visualizzazione dei risultati per i film -->
    <div v-if="movies.length > 0" class="mt-4">
      <h3>Risultati della ricerca - Film:</h3>
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <h4>{{ movie.title }} ({{ movie.original_title }})</h4>
          <p>Lingua originale: <img :src="getFlags(movie.original_language)" alt=""></p>
          <p>Voto: {{ movie.vote_average }}</p>
          <hr>
        </li>
      </ul>
    </div>
    <br>
    <!-- Visualizzazione dei risultati per le serie TV -->
    <div v-if="seriesTv.length > 0" class="mt-4">
      <h3>Risultati della ricerca - Serie TV:</h3>
      <ul>
        <li v-for="serie in seriesTv" :key="serie.id">
          <h4>{{ serie.name }} ({{ serie.original_name }})</h4>
          <p>Lingua originale: <img :src="getFlags(serie.original_language)" alt=""></p>
          <p>Voto: {{ serie.vote_average }}</p>
          <hr>
        </li>
      </ul>
    </div>
  </div>
</template>

<style lang="scss">
@import 'bootstrap/scss/bootstrap';
p >img{
  width: 32px;
  height: 32px;
  object-fit: contain;
}
</style>
