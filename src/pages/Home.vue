<template>
  <div>
    <div class="search">
      <form v-on:submit="getSearchResults">
        <input @input="handleChange" :name="searchQuery" :value="searchQuery">
        <button>Search</button>
      </form>
      <h2>Search Results</h2>
      <section class="search-results container-grid"></section>
    </div>
    <div v-if="!searched" class="genres">
      <h2>Genres</h2>
      <section class="container-grid">
        <GenreCard v-on:click.native="(e)=>selectGenre(genre.id)" v-for="(genre, index) in genres" :genre="genre" :key="index" :index="index"/>
      </section>
    </div>
    <div class="search">
      <h2>Results</h2>
      <section class="container-grid">
        <GameCard v-on:click.native="(e)=>selectGame(game.id)" v-for="game in searchResults" :game="game" :key="game.id" />
      </section>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import GenreCard from '../components/GenreCard.vue'
import GameCard from '../components/GameCard.vue'
const API_KEY = process.env.VUE_APP_KEY
export default {
  name: 'Home',
  components: {
    GenreCard,
    GameCard
  },
  data: () => ({
    genres: [],
    searchQuery: '',
    searchResults: [],
    searched: false
  }),
  mounted: function() {
    this.getGenres()
  },
  methods: {
    async getGenres() {
      const res = await axios.get(`https://api.rawg.io/api/genres?key=${API_KEY}`)
      this.genres = res.data.results
    },
    async getSearchResults(e) {
      e.preventDefault()
      const res = await axios.get(`https://api.rawg.io/api/games?search=${this.searchQuery}&key=${API_KEY}`)
      this.searchResults = res.data.results
      this.searched = true
    },
    handleChange(event) {
      this.searchQuery = event.target.value
    },
    selectGame(gameId) {
      this.$router.push(`/details/${gameId}`)
    },
    selectGenre(genreId) {
      this.$router.push(`/games/${genreId}`)
    }
  }
}
</script>
