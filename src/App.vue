<script>
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return {
      query: '', // testo inserito nella search bar
      movies: [], // array per memorizzare i film trovati
      seriesTv: [], // array per memorizzare le serie tv trovate
      hovered: null
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
    getFlags(lang) {
      const validlangs = [
        'en',
        'it',
        'ja'
      ];
      if (validlangs.includes(lang)) {
        if (lang == 'en') {
          return '/img/flags/uk-flag.gif';
        }
        else if (lang == 'it') {
          return '/img/flags/it-flag.gif';
        }
        else if (lang == 'ja') {
          return '/img/flags/ja-flag.gif';
        }
      } else {
        return '/img/flags/Flag_with_question_mark.svg.png';
      }
    },
    //metodo per convertire il voto in stelle da 1 a 10 --> 1 a 5 arrotondando per eccesso
    convertToStars(vote) {
      return Math.ceil(vote / 2);
    },
  },
};
</script>

<template>
  <div class="container">
    <!-- Header -->
    <AppHeader />
    <div class="searchbar">
      <!-- Search Bar e Button -->
      <form class="row my-2 my-lg-0 justify-content-between" @submit.prevent="searchAll">
        <input v-model="query" class="rounded col-10 mr-sm-2 border-0" type="search" placeholder="Search"
          aria-label="Search" />
        <button class="col-1 btn btn-outline-success my-2 my-sm-0" type="submit">
          Search
        </button>
      </form>
    </div>

    <div class="container">
      <!-- Visualizzazione dei risultati per i film -->
      <div v-if="movies.length > 0" class="mt-4">
        <h3 class="text-white">Risultati della ricerca - Film: {{movies.length}} !</h3>
        <div class="py-2">
          <ul class="row justify-content-center">
            <li class="col-2 text-white rounded border border-white m-2" v-for="movie in movies" :key="movie.id"
              @mouseover="hovered = movie.id" @mouseleave="hovered = null">
              <!--Mostra immagine e titolo -->
              <div v-if="hovered !== movie.id">
                <img :src="'https://image.tmdb.org/t/p/w185/' + movie.poster_path" alt="Movie Poster">
                <h4>{{ movie.title }}</h4>
              </div>

              <!--Mostra dettagli al hover -->
              <div v-if="hovered === movie.id">
                <h4>{{ movie.title }}</h4>
                <h6>({{ movie.original_title }})</h6>
                <p>Lingua originale: <img :src="getFlags(movie.original_language)" alt=""></p>
                <p>
                  <span v-for="n in 5" :key="n">
                    <i v-if="n <= convertToStars(movie.vote_average)" class="fa-solid fa-star text-warning"></i>
                    <i v-else class="fa-regular fa-star"></i>
                  </span>
                </p>
                <p>Overview: {{ movie.overview }}</p>
              </div>
            </li>

          </ul>
        </div>
      </div>
    </div>

    <div>
      <!-- Visualizzazione dei risultati per le serie TV -->
      <div v-if="seriesTv.length > 0" class="mt-4">
        <h3 class="text-white justify">Risultati della ricerca - Serie-TV: {{seriesTv.length}} !</h3>
        <ul class="row justify-content-center">
          <li class="col-2 text-white rounded border border-white m-2" v-for="serie in seriesTv" :key="serie.id" @mouseover="hovered = serie.id" @mouseleave="hovered = null">
            <img :src="'https://image.tmdb.org/t/p/w185/' + serie.poster_path" alt="Serie Poster">
            <h4>{{ serie.name }}</h4>
            <!--Mostra dettagli al hover -->
            <div v-if="hovered === serie.id">
              <h4>{{ serie.name }}</h4>
              <h6>({{ serie.original_name }})</h6>
              <p>Lingua originale: <img :src="getFlags(serie.original_language)" alt=""></p>
              <p>
                <span v-for="n in 5" :key="n">
                  <i v-if="n <= convertToStars(serie.vote_average)" class="fa-solid fa-star text-warning"></i>
                  <i v-else class="fa-regular fa-star"></i>
                </span>
              </p>
              <p>Overview: {{ serie.overview }}</p>
            </div>
            <hr>
          </li>
        </ul>
      </div>
    </div>
  </div>

</template>
<style lang="scss">
@import 'bootstrap/scss/bootstrap';

body {
  background-color: rgb(70, 9, 70);
}

ul>li {
  list-style: none;

}

p>img {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

div {
  transition: opacity 0.3s ease;
}
</style>
