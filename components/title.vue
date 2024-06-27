<template>
  <div></div>
</template>

<script>
export default {
  props: {
    pageTitle: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      title: null
    };
  },
  async asyncData({ $axios }) {
    try {
      const response = await $axios.$get('/api/web/headers');
      const data = response.data;
      return { title: data && data.length > 0 ? data[0].title : null };
    } catch (error) {
      console.error('Error fetching title:', error);
      return { title: null };
    }
  },
  mounted() {
    this.fetchTitle();
  },
  methods: {
    async fetchTitle() {
      try {
        const response = await this.$axios.$get('/api/web/headers');
        const data = response.data;
        this.title = data && data.length > 0 ? data[0].title || this.pageTitle : this.pageTitle;
      } catch (error) {
        console.error('Error fetching title:', error);
        this.title = this.pageTitle;
      }
      this.updateDocumentTitle();
    },
    updateDocumentTitle() {
      if (process.client) {
        document.title = `${this.pageTitle} - ${this.title || ''}`;
      }
    }
  }
};
</script>

<style scoped>
/* Tambahkan gaya CSS jika diperlukan */
</style>
