<template>
  <div>
    <div class="genres">
      <h2>Games By Genre</h2>
      <div class="sort">
        <h3>Sort by Metacritic Reviews</h3>
        <select v-on:change="handleChange">
          <option value=""></option>
          <option value="ascending">Ascending</option>
          <option value="descending">Descending</option>
        </select>
      </div>
    </div>
    <section class="container-grid">
      <GameCard v-on:click.native="(e)=>selectGame(game.id)" v-for="game in games" :game="game" :key="game.id" />
    </section>
    <div class="pageButtons">
      <button @click="changePage(-1)">Previous Page</button>
      <h3>Page {{ this.currentPage }}</h3>
      <button @click="changePage(1)">Next Page</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
const API_KEY = process.env.VUE_APP_KEY
import GameCard from '../components/GameCard.vue'
export default {
  name: 'ViewGames',
  components: {
    GameCard
  },
  data: () => ({
    games: [],
    genre: null,
    sortDir: null,
    currentPage: 1
  }),
  mounted() {
    this.getGamesByGenre()
    // this.getGenre()
  },
  methods: {
    async getGamesByGenre() {
      let genreId = parseInt(this.$route.params.genre_id)
      if (!this.sortDir) {
        const res = await axios.get(`https://api.rawg.io/api/games?genres=${genreId}&page=${this.currentPage}&key=${API_KEY}`)
        this.games = res.data.results
      }
      if (this.sortDir === 'ascending') {
        const res = await axios.get(`https://api.rawg.io/api/games?genres=${genreId}&ordering=metacritic&page=${this.currentPage}&key=${API_KEY}`)
        this.games = res.data.results
      }
      else if (this.sortDir === 'descending') {
        const res = await axios.get(`https://api.rawg.io/api/games?genres=${genreId}&ordering=-metacritic&page=${this.currentPage}&key=${API_KEY}`)
        this.games = res.data.results
      }
    },
    selectGame(gameId) {
      this.$router.push(`/details/${gameId}`)
    },
    sortGames(dir) {
      if (dir === 'asc') this.games.sort(({metacritic:a}, {metacritic:b}) => b-a)
      console.log(this.games)
    },
    async handleChange(e){
      this.sortDir = e.target.value
      this.getGamesByGenre()
    },
    changePage(dir){
      this.currentPage += dir
      this.getGamesByGenre()
    }
    // async getGenre(){
    //   let genreId = parseInt(this.$route.params.genre_id)
    //   const res = await axios.get(`https://api.rawg.io/api/genres=${genreId}&key=${API_KEY}`)
    //   this.genre = res.data.results
    // }
  }
}
</script>
