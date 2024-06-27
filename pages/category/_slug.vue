<template>
  <b-container class="mt-5 mb-5">
    <!-- Bagian Kategori -->
    <b-row>
      <b-col md="12" class="mb-3">
        <h4>KATEGORI : <strong>{{ category.name ? category.name.toUpperCase() : '' }}</strong></h4>
      </b-col>
    </b-row>

    <!-- Bagian Blog -->
    <b-row v-if="blogs.length">
      <b-col md="4" class="mb-3" sm="12" v-for="blog in blogs" :key="blog.id">
        <b-card :img-src="blog.image" img-top tag="article" class="mb-2 h-100 rounded-lg">
          <div class="mb-2">
            <small class="text-muted"><i class="fa fa-calendar"></i> {{ blog.created_at }}</small>
          </div>
          <h5>
            <nuxt-link :to="{name: 'blog-slug', params: {slug: blog.slug}}">{{ blog.title }}</nuxt-link>
          </h5>
          <b-card-text>
            {{ blog.description ? blog.description.substr(0, 55) : '' }}...
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>

  </b-container>
</template>

<script>
export default {
  head() {
    return {
      title: `${this.category.name} - Blog`,
      meta: [
        { hid: 'og:title', name: 'og:title', content: this.category.name },
        { hid: 'og:site_name', name: 'og:site_name', content: this.category.name },
        { hid: 'og:image', name: 'og:image', content: this.category.image },
      ]
    };
  },

  async asyncData({ params, $axios }) {
    try {
      const { data } = await $axios.$get(`/api/web/categories/${params.slug}`);
      return {
        category: data,
        blogs: data.blogs || [],
        galleries: data.galleries || [],
      };
    } catch (error) {
      console.error('Error fetching data:', error);
      return {
        category: {},
        blogs: [],
        galleries: [],
      };
    }
  }
};
</script>

<style>
/* Tambahkan gaya kustom Anda di sini jika diperlukan */
</style>
