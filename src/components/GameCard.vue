<template>
  <div class="card game-card">
    <div class="image-wrapper">
      <img :src='game.background_image' />
    </div>
    <div class="info-wrapper flex-col">
      <p>{{ game.name }}</p>
      <h3>Metacritic: {{ game.metacritic }}</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameCard',
  props: ['game']
}
</script>
