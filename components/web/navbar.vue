<template>
  <div>
    <!-- Navbar -->
    <b-navbar toggleable="lg" type="dark" class="bg-navbar modern-navbar">
      <b-container fluid class="d-flex justify-content-between align-items-center">
        <nuxt-link to="#" class="navbar-brand d-flex align-items-center" v-for="header in headers" :key="header.id">
          <b-img :src="header.image" :alt="header.name" width="30" height="30" class="d-inline-block align-text-top me-2" style="margin-right: 10px;"></b-img>
          <div class="d-inline-block">
            <span class="fw-bold">{{ header.name }}</span>
          </div>
        </nuxt-link>

        <!-- Toggle Button -->
        <b-navbar-toggle target="nav-collapse" class="border-0 custom-toggler ms-auto">
          <span class="navbar-toggler-icon"></span>
        </b-navbar-toggle>

        <!-- Navbar Collapse -->
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ms-3">
            <b-nav-item to="/" class="nav-link">
              <i class="fas fa-home me-2"></i>Beranda
            </b-nav-item>
            <b-nav-item to="/anggota" class="nav-link">
              <i class="fas fa-users me-2"></i>Anggota
            </b-nav-item>
            <b-nav-item to="/about" class="nav-link">
              <i class="fas fa-info-circle me-2"></i>About
            </b-nav-item>
            <b-nav-item to="/blog" class="nav-link">
              <i class="fas fa-blog me-2"></i>Blog
            </b-nav-item>
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
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll() {
      const navbar = document.querySelector('.bg-navbar');
      if (window.scrollY > 50) { // Change '50' to the value where you want the navbar to become sticky
        navbar.classList.add('sticky');
      } else {
        navbar.classList.remove('sticky');
      }
    }
  }
};
</script>


<style scoped>
.bg-navbar {
  background-color: #189954; /* Warna background navbar */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Box shadow for modern look */
  transition: all 0.3s ease; /* Smooth transitions */
}
.modern-navbar {
  background: linear-gradient(45deg, #189954, #1a7a48); /* Subtle gradient */
}
.navbar-brand, .navbar-nav .nav-link {
  color: white !important; /* Warna teks putih */
  transition: color 0.3s, transform 0.3s; /* Smooth transitions */
}
.navbar-brand:hover, .navbar-nav .nav-link:hover {
  color: #cce7d0 !important; /* Slightly lighter hover effect */
  transform: scale(1.05); /* Slightly enlarge on hover */
}
.navbar-brand .fw-bold {
  color: white !important; /* Warna teks nama header */
}
.custom-toggler.navbar-toggler {
  border-color: white !important; /* Mengubah warna border toggler menjadi putih */
}
.custom-toggler.navbar-toggler .navbar-toggler-icon {
  background-color: transparent; /* Menghapus background image default */
  width: 30px;
  height: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  padding: 5px;
}
.custom-toggler.navbar-toggler .navbar-toggler-icon span {
  display: block;
  width: 100%;
  height: 2px;
  background-color: white;
}

.nav-link i {
  margin-right: 0.5rem; /* Margin for icons */
}

@media (min-width: 992px) {
  .navbar-brand {
    margin-left: 10rem; /* Menambahkan margin kiri pada logo dan nama header */
  }
}

/* New styles for sticky navbar */
.sticky {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000; /* Ensure it is above other content */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  animation: slideDown 0.3s ease-in-out; /* Animation for sliding down */
}

@keyframes slideDown {
  from {
    transform: translateY(-100%);
  }
  to {
    transform: translateY(0);
  }
}

body {
  padding-top: 70px; /* Add top padding to body to avoid content being covered by navbar */
}
</style>

