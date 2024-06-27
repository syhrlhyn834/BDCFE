<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Header'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title">HEADER WEBSITE</h3>
          <div class="card-tools">
            <button type="button" class="btn btn-tool" data-card-widget="collapse" title="Collapse">
              <i class="fas fa-minus"></i>
            </button>
            <button type="button" class="btn btn-tool" data-card-widget="remove" title="Remove">
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
        <div class="card-body">
          <b-table striped bordered hover :items="headers" :fields="fields" show-empty>
            <template v-slot:cell(image)="data">
              <img class="img-fluid" width="100" :src="data.item.image" />
            </template>
            <template v-slot:cell(actions)="row">
              <b-button :to="{name: 'admin-header-edit-id', params: {id: row.item.id}}" variant="warning" size="sm">EDIT</b-button>
            </template>
          </b-table>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Title from '@/components/title.vue';

export default {
  layout: 'admin',
  components: {
  },


  async asyncData({ $axios }) {
    try {
      const response = await $axios.$get('/api/admin/headers');
      return { headers: response.data.data };
    } catch (error) {
      console.error('Error fetching data:', error);
      return { headers: [] };
    }
  },
  data() {
    return {
      fields: [
        { label: 'Logo Website', key: 'image' },
        { label: 'Title Website', key: 'title' },
        { label: 'Name Website', key: 'name' },
        { label: 'Deskripsi Website', key: 'description' },
        { label: 'Actions', key: 'actions', tdClass: 'text-center' }
      ]
    }
  }
}
</script>

<style>
  /* tambahkan gaya CSS di sini jika diperlukan */
</style>
