<template>
  <footer class="footer-top pt-4">
    <b-container>
      <b-row>
        <b-col md="4">
          <div v-for="header in headers" :key="header.id">
            <h5>{{ header.name }}</h5>
          </div>
          <hr>
          <p v-for="footer in footers" :key="footer.id">{{ footer.description }}</p>
        </b-col>
        <b-col md="4">
          <h5>SOSIAL MEDIA</h5>
          <hr>
          <div>
            <p v-for="sosmed in sosmeds" :key="sosmed.id">
              <a :href="getSosmedUrl(sosmed)" class="d-inline-flex align-items-center text-decoration-none" style="color: white;">
                <i :class="sosmed.logo" style="width: 24px; height: 24px; margin-right: 5px;"></i>
                {{ sosmed.name }}
              </a>
            </p>
          </div>
        </b-col>
        <b-col md="4">
          <h5>ALAMAT</h5>
          <hr>
          <p><i aria-hidden="true" class="fa fa-map-marker"></i> Jl. Jendral Sudirman No.83, Daerah Khusus
            Ibukota Jakarta, Indonesia</p>
        </b-col>
      </b-row>
    </b-container>
    <b-container fluid class="footer-bottom">
      <b-row class="p-3 justify-content-center">
        <div class="text-white font-weight-bold" v-for="footer in footers" :key="footer.id">{{ footer.name }}</div>
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
      const [ footersResponse, sosmedsResponse, headersResponse] = await Promise.all([
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
</style>
