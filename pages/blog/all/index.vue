<template>
  <b-container class="mt-5 mb-5">
    <Title :pageTitle="'All Blog'" />
    <b-row>
      <b-col md="4" sm="12" v-for="blog in blogs" :key="blog.id">
        <b-card :img-src="blog.image" img-top tag="article" class="mb-2 h-100 rounded-lg">
          <div class="mb-2">
            <small class="text-muted"><i class="fa fa-calendar"></i> {{ blog.created_at }}</small>
          </div>
          <h5>
            <nuxt-link :to="{name: 'blog-slug', params: {slug: blog.slug}}">{{ blog.title }}</nuxt-link>
          </h5>
          <b-card-text>
            {{ blog.description.substr(0, 55) }}...
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>
    <b-row class="mt-4 justify-content-center">
      <b-pagination v-model="pagination.current_page" :total-rows="pagination.total" :per-page="pagination.per_page"
                    @input="changePage" aria-controls="my-table">
      </b-pagination>
    </b-row>
  </b-container>
</template>

<script>
import Title from '@/components/title.vue';

export default {
  components: {
    Title
  },
  watchQuery: ["page"],
  async asyncData({ $axios, query }) {
    let page = query.page ? parseInt(query.page) : ''
    const blogs = await $axios.$get(`/api/web/blogs?page=${page}`)
    return {
      'blogs': blogs.data.data,
      'pagination': blogs.data
    }
  },
  data() {
    return {
      search: '',
      pagination: {}
    }
  },
  methods: {
    changePage(page) {
      this.$router.push({
        path: this.$route.path,
        query: {
          page: page
        }
      });
    }
  }
}
</script>

<style scoped>

</style>
