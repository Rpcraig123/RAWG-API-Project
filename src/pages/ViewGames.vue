<template>
  <div>
    <div class="genres">
      <h2>Games By Genre</h2>
      <select>
        <option value="ascending">Ascending</option>
        <option value="descending">Descending</option>
      </select>
    </div>
    <section class="container-grid">
      <GameCard v-on:click.native="(e)=>selectGame(game.id)" v-for="game in games" :game="game" :key="game.id" />
    </section>
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
    genre: null
  }),
  mounted() {
    this.getGamesByGenre()
  },
  methods: {
    async getGamesByGenre() {
      let genreId = parseInt(this.$route.params.genre_id)
      const res = await axios.get(`https://api.rawg.io/api/games?genres=${genreId}&key=${API_KEY}`)
      console.log(res)
      this.games = res.data.results
    },
    selectGame(gameId) {
      this.$router.push(`/details/${gameId}`)
    }
  }
}
</script>
