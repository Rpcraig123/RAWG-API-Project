<template>
  <div class="game-content">
    <section class="image-container">
      <img :src='gameDetails.background_image' />
    </section>
    <section class="details">
      <div class="flex-row space">
        <h3>{{ gameDetails.name }}</h3>
      </div>
      <button @click="returnHome">Back</button>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
const API_KEY = process.env.VUE_APP_KEY
export default {
  name: 'GameDetails',
  components: {},
  data: () => ({
    gameDetails: null
  }),
  mounted: function() {
    this.getGameDetails()
  },
  methods: {
    async getGameDetails() {
      let gameId = parseInt(this.$route.params.game_id)
      const res = await axios.get(`https://api.rawg.io/api/games/${gameId}?key=${API_KEY}`)
      this.gameDetails = res.data
      console.log
    },
    returnHome() {
      this.$router.push(`/`)
    }
  }
}
</script>
