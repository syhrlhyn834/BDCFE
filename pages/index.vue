<template>
  <div class="homepage vh-100 d-flex flex-column justify-content-center align-items-center">
    <Title :pageTitle="'Beranda'" />
    <div class="video-background position-absolute w-100 h-100">
      <!-- Replace iframe with image -->
      <img src="https://t4.ftcdn.net/jpg/06/00/90/17/360_F_600901739_93i8HBQhHjqXyvNBrHWEiJOveQYdMe6m.jpg" alt="Background Image" class="embed-responsive-item" />
    </div>
    <div class="overlay position-absolute w-100 h-100 d-flex align-items-center justify-content-center">
      <div class="text-center" v-for="header in headers" :key="header.id">
        <h1 class="display-4">{{ header.description }}</h1>
        <p class="lead">{{ header.name }}</p>
        <b-button to="/about" variant="primary" class="mt-4">About</b-button>
      </div>
    </div>
  </div>
</template>

<script>
import Title from '@/components/title.vue';
export default {
  data() {
    return {
      headers: []
    };
  },
  async fetch() {
    try {
      const headersResponse = await this.$axios.get('/api/web/headers');
      this.headers = headersResponse.data.data;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }
};
</script>

<style scoped>
.homepage {
  position: relative;
  overflow: hidden;
}

.video-background {
  z-index: -1;
}

.embed-responsive-item {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure image covers the entire container */
}

.overlay {
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
}

.display-4 {
  font-size: 2.5rem;
  color: white;
}

.lead {
  font-size: 1.25rem;
  color: white;
}

@media (max-width: 768px) {
  .display-4 {
    font-size: 1.5rem;
  }
  .lead {
    font-size: 1rem;
  }
}
</style>
