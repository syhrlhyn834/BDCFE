<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Sosial Media'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> SOSIAL MEDIA</h3>
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
          <div class="form-group">
            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <nuxt-link :to="{name: 'admin-sosmed-create'}" class="btn btn-info btn-sm" style="padding-top: 8px;">
                  <i class="fa fa-plus-circle"></i> TAMBAH
                </nuxt-link>
              </div>
              <input type="text" class="form-control" v-model="search" @keypress.enter="searchData" placeholder="cari berdasarkan nama/username sosmed">
              <div class="input-group-append">
                <button @click="searchData" class="btn btn-info">
                  <i class="fa fa-search"></i> CARI
                </button>
              </div>
            </div>
          </div>

          <!-- table -->
          <b-table striped bordered hover :items="sosmeds" :fields="fields" show-empty>
            <template v-slot:cell(logo)="data">
              <!-- Pastikan data.item.logo berisi class ikon Bootstrap -->
              <i :class="[data.item.logo, 'large-icon']"></i>
            </template>
            <template v-slot:cell(actions)="row">
              <b-button :to="{name: 'admin-sosmed-edit-id', params: {id: row.item.id}}" variant="warning" size="sm">EDIT</b-button>
              <b-button variant="danger" size="sm" @click="deleteSosmed(row.item.id)">DELETE</b-button>
            </template>
          </b-table>

          <!-- pagination -->
          <b-pagination v-model="pagination.current_page" :total-rows="pagination.total" :per-page="pagination.per_page"
            @change="changePage" align="right" class="mt-3"></b-pagination>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Title from '@/components/title.vue';
export default {
  layout: 'admin',

  data() {
    return {
      fields: [
        { label: 'Logo Sosmed', key: 'logo' },
        { label: 'Name', key: 'name' },
        { label: 'Url', key: 'url' },
        { label: 'Username', key: 'username' },
        { label: 'Actions', key: 'actions', tdClass: 'text-center' }
      ],
      search: ''
    }
  },

  watchQuery: ["q", "page"],

  async asyncData({ $axios, query }) {
    let page = query.page ? parseInt(query.page) : ''
    let search = query.q ? query.q : ''

    const sosmeds = await $axios.$get(`/api/admin/sosmeds?q=${search}&page=${page}`)
    return {
      'sosmeds': sosmeds.data.data,
      'pagination': sosmeds.data
    }
  },

  methods: {
    changePage(page) {
      this.$router.push({ path: this.$route.path, query: { q: this.$route.query.q, page: page } });
    },
    searchData() {
      this.$router.push({ path: this.$route.path, query: { q: this.search } });
    },
    deleteSosmed(id) {
      this.$swal.fire({
        title: 'APAKAH ANDA YAKIN ?',
        text: "INGIN MENGHAPUS DATA INI !",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#3085d6',
        confirmButtonText: 'YA, HAPUS!',
        cancelButtonText: 'TIDAK',
      }).then((result) => {
        if (result.isConfirmed) {
          this.$axios.delete(`/api/admin/sosmeds/${id}`).then(() => {
            this.$nuxt.refresh()
            this.$swal.fire({
              title: 'BERHASIL!',
              text: "Data Berhasil Dihapus!",
              icon: 'success',
              showConfirmButton: false,
              timer: 2000
            })
          })
        }
      })
    }
  }
}
</script>
<style>
.large-icon {
  font-size: 3rem;
}
</style>
