<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Footer'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title">FOOTER WEBSITE</h3>
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
          <!-- table -->
          <b-table striped bordered hover :items="footers" :fields="fields" show-empty>
            <template v-slot:cell(actions)="row">
              <b-button :to="{name: 'admin-footer-edit-id', params: {id: row.item.id}}" variant="warning" size="sm">EDIT</b-button>
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

  async asyncData({ $axios }) {
    try {
      const response = await $axios.$get('/api/admin/footers');
      return { footers: response.data.data };
    } catch (error) {
      console.error('Error fetching data:', error);
      return { footers: [] };
    }
  },

  data() {
    return {
      // table footer
      fields: [
        { label: 'Name Footer', key: 'name' },
        { label: 'Deskripsi Footer', key: 'description' },
        { label: 'Alamat', key: 'alamat'},
        { label: 'Kontak', key: 'kontak'},

        { label: 'Actions', key: 'actions', tdClass: 'text-center' }
      ],

      // state untuk menyimpan data footers
      footers: []
    }
  }
}
</script>

<style>
  /* tambahkan gaya CSS di sini jika diperlukan */
</style>
