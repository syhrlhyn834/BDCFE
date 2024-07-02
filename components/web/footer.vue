<template>
  <footer class="footer-top pt-4">
    <b-container>
      <b-row>
        <b-col md="4" class="footer-section">
          <div v-for="(header, index) in headers" :key="`header-${header.id}-${index}`">
            <h5>{{ header.name }}</h5>
          </div>
          <hr>
          <p v-for="(footer, index) in footers" :key="`footer-description-${footer.id}-${index}`">{{ footer.description }}</p>
        </b-col>
        <b-col md="4" class="footer-section">
          <h5>SOSIAL MEDIA</h5>
          <hr>
          <div>
            <p v-for="(sosmed, index) in sosmeds" :key="`sosmed-${sosmed.id}-${index}`">
              <a :href="getSosmedUrl(sosmed)" class="d-inline-flex align-items-center text-decoration-none" style="color: white;">
                <i :class="sosmed.logo" style="width: 24px; height: 24px; margin-right: 5px;"></i>
                {{ sosmed.name }}
              </a>
            </p>
          </div>
        </b-col>
        <b-col md="4" class="footer-section">
          <h5>ALAMAT</h5>
          <hr>
          <p v-for="(footer, index) in footers" :key="`footer-address-${footer.id}-${index}`">
            <i aria-hidden="true" class="fa fa-map-marker"></i> {{ footer.alamat }}
            <br><br>
            <i class="fas fa-phone"></i> {{ footer.kontak }}
          </p>
        </b-col>
      </b-row>
    </b-container>
    <b-container fluid class="footer-bottom">
      <b-row class="p-3 justify-content-center">
        <div class="text-white font-weight-bold" v-for="(footer, index) in footers" :key="`footer-name-${footer.id}-${index}`">{{ footer.name }}</div>
      </b-row>
    </b-container>
  </footer>
</template>


<script>
export default {
  data() {
    return {
      sosmeds: [],
      footers: [],
      headers: []
    };
  },
  async fetch() {
    try {
      const [footersResponse, sosmedsResponse, headersResponse] = await Promise.all([
        this.$axios.get('/api/web/footers'),
        this.$axios.get('/api/web/sosmeds'),
        this.$axios.get('/api/web/headers')
      ]);
      this.footers = footersResponse.data.data;
      this.sosmeds = sosmedsResponse.data.data;
      this.headers = headersResponse.data.data;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  },
  methods: {
    getSosmedUrl(sosmed) {
      return `${sosmed.url}/${sosmed.username}`;
    }
  }
};
</script>

<style scoped>
/* Styling khusus untuk komponen ini */
.footer-section {
  margin-bottom: 2.5rem; /* Adjust the space between sections */
}

</style>
