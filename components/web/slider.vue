<template>
  <div>
    <b-container class="mt-3">
      <b-carousel id="carousel-fade" style="text-shadow: 0px 0px 2px #000" fade indicators img-width="1024" img-height="300">
        <b-carousel-slide v-for="slider in sliders" :key="slider.id" :img-src="slider.image" class="carousel-image">
        </b-carousel-slide>
      </b-carousel>
    </b-container>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        sliders: []
      }
    },
    async fetch() {
      await this.$axios.get('/api/web/sliders')
      .then(response => {
        this.sliders = response.data.data;
      })
    }
  }
</script>

<style>
.carousel-image img {
  width: 100%;
  height: 300px;
  object-fit: cover; /* Ensures the image covers the container while maintaining its aspect ratio */
}
</style>
