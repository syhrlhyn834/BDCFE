<template>
  <b-container class="mt-5 mb-5">
    <b-row class="mb-3">
      <b-col md="8" sm="12">
        <b-form-input v-model="search" @keypress.enter="searchData" size="sm" class="border-0"
                      placeholder="tulis kata kunci..." style="margin-bottom: 10px;">
        </b-form-input>
      </b-col>
      <b-col md="4" sm="12">
        <b-button @click="searchData" size="sm" class="my-2 my-sm-0" variant="primary" style="margin-bottom: 10px;">CARI</b-button>
      </b-col>
    </b-row>
    <b-row>
      <b-col md="12" class="mb-3">
        <h4>PENCARIAN DENGAN KATA KUNCI : <strong>{{ $route.query.q }}</strong></h4>
      </b-col>
      <b-col md="4" class="mb-3" sm="12" v-for="blog in blogs" :key="'blog-' + blog.id">
        <b-card :img-src="blog.image" img-top tag="article" class="mb-2 h-100 rounded-lg">
          <div class="mb-2">
            <small class="text-muted"><i class="fa fa-calendar"></i> {{ blog.created_at }}</small>
          </div>
          <h5>
            <nuxt-link :to="{ name: 'blog-slug', params: { slug: blog.slug } }">{{ blog.title }}</nuxt-link>
          </h5>
          <b-card-text>
            {{ blog.description.substr(0, 55) }}...
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>



<script>
export default {
  //meta
  head() {
    return {
      title: 'Search - Blog',
      meta: [
        {
          hid: 'og:title',
          name: 'og:title',
          content: 'Blog'
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          content: 'Blog'
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: 'https://cdn.jsdelivr.net/gh/maulayyacyber/assets-images-ebooks/cms-nuxt-laravel-11/xKOCz0P.png'
        },
      ]
    }
  },

  //watch query URL
  watchQuery: ["q"],

  async asyncData({ $axios, query }) {
    try {
      // Fetching blogs
      const blogsResponse = await $axios.$get(`/api/web/blogs?q=${query.q}`);
      const blogs = blogsResponse.data.data;


    } catch (error) {
      console.error('Error fetching data:', error);
      return {
        blogs: []
      };
    }
  },
};
</script>


<style>

</style>
