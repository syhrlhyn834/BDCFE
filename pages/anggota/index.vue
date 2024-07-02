<template>
  <b-container class="mt-5 mb-5">
    <Title :pageTitle="'Daftar Anggota'" />
    <b-row class="card-row">
      <b-col md="3" sm="6" v-for="anggota in anggotas" :key="anggota.id" class="d-flex">
        <b-card v-if="!loading" class="flex-fill mb-4">
          <b-card-img :src="anggota.image" alt="Image" top class="card-img"></b-card-img>
          <b-card-body>
            <b-card-text>
              <div class="name"><strong>{{ anggota.name }}</strong></div>
              <div class="posisi text-black">{{ anggota.posisi }}</div>
            </b-card-text>
            <nuxt-link :to="{ name: 'anggota-slug', params: { slug: anggota.slug } }">
              <b-button variant="primary">Selengkapnya...</b-button>
            </nuxt-link>
          </b-card-body>
        </b-card>
        <b-card v-else class="flex-fill mb-4">
          <b-card-body>
            <b-card-text>
              <div v-if="error" class="error-message">{{ error }}</div>
              <div v-else class="loading-message">Loading...</div>
            </b-card-text>
          </b-card-body>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Title from '@/components/title.vue';
export default {
  async asyncData({ $axios }) {
    try {
      const response = await $axios.$get('/api/web/anggotas');
      console.log('Fetched data:', response.data.data); // Log the response to check for duplicates
      return { anggotas: response.data.data, loading: false, error: null };
    } catch (error) {
      console.error('Error fetching data:', error); // Log errors
      return { anggotas: [], loading: false, error: 'Error fetching data' };
    }
  },
};
</script>

<style scoped>
.text-black {
  color: black;
}

.card-row {
  display: flex;
  flex-wrap: wrap;
}

.b-col.d-flex {
  display: flex;
  flex-direction: column;
}

.b-card.flex-fill {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  justify-content: space-between;
}

.mb-4 {
  margin-bottom: 1.5rem;
}

.card-img {
  height: 250px;
  width: 100%;
  object-fit: cover;
}

.name {
  font-size: 1rem;
  margin-bottom: 0.5rem;
}

.posisi {
  font-size: 0.875rem;
  margin-bottom: 1rem;
}

.loading-message, .error-message {
  font-size: 1rem;
  text-align: center;
}

@media (max-width: 768px) {
  .name, .posisi {
    font-size: 0.875rem;
    margin-bottom: 0.5rem;
  }
}
</style>
