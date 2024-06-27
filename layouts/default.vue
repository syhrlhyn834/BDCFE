<template>
  <div>
    <!-- navbar -->
    <Navbar />

    <!-- content -->
    <Nuxt />

    <!-- footer -->
    <Footer />

  </div>
</template>
<script>
  import Navbar from '@/components/web/navbar.vue'
  import Footer from '@/components/web/footer.vue'
  export default {
    components: {
      Navbar,
      Footer
    },
  }
</script>

<style>
  body {
    background-color: #ccdcec !important;
  }


  .btn-primary {
    background-color: #189954;
    border-color: #189954;
  }

  .btn-primary:hover {
    background-color: #189954;
    border-color: #189954;
  }

  .btn-primary-dark {
    background-color: #189954;
    border-color: #189954;
    color: white;
  }

  .btn-primary-dark:hover {
    color: white;
  }

  .carousel-item .img-fluid {
    border-radius: .5rem;
  }

  .card-category {
    background-color: #dadada;
  }

  .object-fit-cover {
    object-fit: cover;
  }

  .footer-top {
    background-color: #189954;
    color: white;
  }

  .footer-bottom {
    background-color: #158549;
  }
</style>

