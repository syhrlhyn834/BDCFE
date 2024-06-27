<template>
  <div>
    <Title :pageTitle="'Blog'" />
    <!-- slider -->
    <Slider />

    <b-container class="mt-5 mb-5">
      <b-row>

        <b-col md="8" sm="12">
          <b-card no-body class="border-0 rounded-lg shadow-md mb-3" v-for="blog in blogs" :key="blog.id">
            <b-row class="g-0">
              <b-col md="4">
                <b-img :src="blog.image" class="w-100 img-post h-100 object-fit-cover" rounded="left"></b-img>
              </b-col>
              <b-col md="8">
                <b-card-body>
                  <h5>
                    <nuxt-link :to="{name: 'blog-slug', params: {slug: blog.slug}}">{{ blog.title }}</nuxt-link>
                  </h5>
                  <b-card-text> {{ blog.description.substr(0, 55) }}...</b-card-text>
                  <b-card-text>
                    <small class="text-muted mr-3"><i class="fa fa-calendar"></i> {{ blog.created_at }}</small>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
          <b-row class="mt-5">
            <b-col md="12" class="text-center">
              <nuxt-link to="/blog/all" class="btn btn-md btn-primary rounded shadow-sm">
                LIHAT LEBIH BANYAK
              </nuxt-link>
            </b-col>
          </b-row>
        </b-col>
        <b-col md="4" sm="12">
          <b-card no-body class="border-0 rounded-lg shadow-md">
            <b-card-body>
              <h5>KATEGORI</h5>
              <hr>
              <b-card no-body class="border-0 shadow-sm card-category mb-2 rounded-lg"
                v-for="category in categories" :key="category.id">
                <b-card-body class="p-2 fw-normal">
                  <b-img :src="category.image" width="40"></b-img>
                  <nuxt-link :to="{name: 'category-slug', params: {slug: category.slug}}" class="text-dark">
                    {{ category.name }}</nuxt-link>
                </b-card-body>
              </b-card>
            </b-card-body>
          </b-card>


        </b-col>
      </b-row>
    </b-container>


  </div>
</template>

<script>
  import Title from '@/components/title.vue';
  import Slider from '@/components/web/slider.vue';

  export default {

    components: {
      Slider
    },

    async asyncData({ $axios }) {

      //fetching blogs
      const blogs = await $axios.$get('/api/web/blogHomepage')

      //fetching categories
      const categories = await $axios.$get('/api/web/categorySidebar')



      return {
        'blogs': blogs.data,
        'categories': categories.data
      }
    }

  }
</script>

<style>
</style>
