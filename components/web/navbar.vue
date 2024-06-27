<template>
  <div>
    <!-- Navbar -->
    <b-navbar toggleable="lg" type="dark" class="bg-navbar">
      <b-container fluid>
        <nuxt-link to="#" class="navbar-brand" v-for="header in headers" :key="header.id">
          <b-img :src="header.image" :alt="header.name" width="30" height="30" class="d-inline-block align-text-top"></b-img>
          <div class="d-inline-block ms-2">
            <span class="fw-bold">{{ header.name }}</span>
          </div>
        </nuxt-link>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item to="/" class="nav-link">Beranda</b-nav-item>
            <b-nav-item to="/anggota" class="nav-link">Anggota</b-nav-item>
            <b-nav-item to="/about" class="nav-link">About</b-nav-item>
            <b-nav-item to="/blog" class="nav-link">Blog</b-nav-item>
            <b-nav-item to="/contact" class="nav-link">Contact</b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
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
.bg-navbar {
  background-color: #189954; /* Warna background navbar */
}
.navbar-brand, .navbar-nav .nav-link {
  color: white !important; /* Warna teks putih */
}
.navbar-brand .fw-bold {
  color: white !important; /* Warna teks nama header */
}
</style>
