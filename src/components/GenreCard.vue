<template>
  <div class="card">
    <div class="image-wrapper">
      <img :src='genre.image_background' />
    </div>
    <div class="info-wrapper flex-col">
      <p>{{ genre.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GenreCard',
  props: ['genre', 'index']
}
</script>
