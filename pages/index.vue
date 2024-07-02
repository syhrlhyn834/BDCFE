<template>
  <div class="homepage vh-100 d-flex flex-column justify-content-center align-items-center">
    <Title :pageTitle="'Beranda'" />
    <div class="background-container position-absolute w-100 h-100" v-for="header in headers" :key="header.id">
      <img :src="header.image2" alt="Background Image" class="background-image" />
    </div>
    <div class="overlay position-absolute w-100 h-100 d-flex align-items-center justify-content-center">
      <transition name="page">
        <div class="text-center">
          <h1 class="display-4">{{ headers.length > 0 ? headers[0].description : '' }}</h1>
          <p class="lead">{{ headers.length > 0 ? headers[0].name : '' }}</p>
          <b-button to="/about" variant="primary" class="mt-4">About</b-button>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import Title from '@/components/title.vue';

export default {
  components: {
    Title
  },
  data() {
    return {
      headers: []
    };
  },
  async mounted() {
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

.background-container {
  z-index: -1;
}

.background-image {
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
