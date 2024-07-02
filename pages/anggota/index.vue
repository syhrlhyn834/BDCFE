<template>
  <b-container class="mt-5 mb-5">
    <b-row class="card-row">
      <b-col md="3" sm="6" v-for="anggota in anggotas" :key="anggota.id" class="d-flex" v-if="anggotas.length > 0">
        <b-card
          :img-src="anggota.image"
          img-alt="Image"
          img-top
          class="flex-fill mb-4"
          style="height: 400px;">
          <b-card-text>
            <div class="name"><strong>{{ anggota.name }}</strong></div>
            <div class="posisi text-black">{{ anggota.posisi }}</div>
          </b-card-text>
          <nuxt-link :to="{name: 'anggota-slug', params: {slug: anggota.slug}}">
            <b-button variant="primary">Selengkapnya...</b-button>
          </nuxt-link>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      anggotas: []
    };
  },
  async created() {
    try {
      const response = await this.$axios.$get('/api/web/anggotas');
      this.anggotas = response.data.data;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }
}

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

.name {
  font-size: 1rem;
  margin-bottom: 0.5rem;
}

.posisi {
  font-size: 0.875rem;
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .name, .posisi {
    font-size: 0.875rem;
    margin-bottom: 0.5rem;
  }
}
</style>
