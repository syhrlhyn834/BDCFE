<template>
  <b-container class="mt-5 mb-5">
    <b-row>
      <b-col md="8" sm="12">
        <b-card class="border-0 rounded-lg shadow-md mb-3">
          <b-img :src="blog.image" class="rounded-lg w-100 card-img"></b-img>
          <hr>
          <h4>{{ blog.title }}</h4>
          <b-card-text>
            <small class="text-muted mr-3"><i class="fa fa-calendar"></i> {{ blog.created_at }} </small>
            <small class="text-muted mr-3"><i class="fa fa-user"></i> {{ blog.user.name }}</small>
            <small class="text-muted mr-3"><i class="fa fa-folder"></i> {{ blog.category.name }}</small>
          </b-card-text>
          <div v-html="blog.content" class="card-content"></div>
          <hr>
          <h5>
            KATEGORI:
            <nuxt-link
              :to="{ name: 'category-slug', params: { slug: blog.category.slug } }"
              class="btn btn-info btn-sm mb-2 shadow-sm"
            >
              {{ blog.category.name }}
            </nuxt-link>
          </h5>
        </b-card>
      </b-col>

      <b-col md="4" sm="12">
        <b-card no-body class="border-0 rounded-lg shadow-md">
          <b-card-body>
            <h5>KATEGORI</h5>
            <hr>
            <b-card no-body class="border-0 shadow-sm card-category mb-2 rounded-lg" v-for="category in categories"
              :key="category.id">
              <b-card-body class="p-2 fw-normal">
                <b-img :src="category.image" width="40" class="card-category-img"></b-img>
                <nuxt-link :to="{name: 'category-slug', params: {slug: category.slug}}" class="text-dark">{{ category.name }}</nuxt-link>
              </b-card-body>
            </b-card>
          </b-card-body>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
    head() {
      return {
        title: this.blog.title,
        meta: [
          {
            hid: 'og:title',
            name: 'og:title',
            content: this.blog.title
          },
          {
            hid: 'og:site_name',
            name: 'og:site_name',
            content: this.blog.title
          },
          {
            hid: 'og:image',
            name: 'og:image',
            content: this.blog.image
          },
          {
            hid: 'description',
            name: 'description',
            content: this.blog.description
          }
        ]
      }
    },
    async asyncData({ params, $axios }) {
      const blog = await $axios.$get(`/api/web/blogs/${params.slug}`);
      const categories = await $axios.$get('/api/web/categorySidebar');
      return {
        'blog': blog.data,
        'categories': categories.data,
      }
    }
  }
</script>

<style>
.card-img {
  max-width: 100%;
  height: auto;
  object-fit: cover;
}

.card-content img {
  max-width: 100%;
  height: auto;
}

.card-category-img {
  max-width: 40px;
  height: auto;
}
</style>
