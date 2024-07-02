<template>
  <div class="content-wrapper">
    <section class="content-footer">
      <div class="container-fluid">
        <Title :pageTitle="'Edit Footer'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-footer">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> EDIT FOOTERS</h3>
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
          <form @submit.prevent="updateFooter">
            <div class="form-group">
              <label>NAME FOOTER</label>
              <input type="text" v-model="footer.name" placeholder="Masukkan Nama Website" class="form-control">
              <div v-if="validation.name" class="mt-2">
                <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>DESKRIPSI FOOTER</label>
              <input type="text" v-model="footer.description" placeholder="Masukkan Description" class="form-control">
              <div v-if="validation.description" class="mt-2">
                <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>ALAMAT FOOTER</label>
              <input type="text" v-model="footer.alamat" placeholder="Masukkan Alamat" class="form-control">
              <div v-if="validation.alamat" class="mt-2">
                <b-alert show variant="danger">{{ validation.alamat[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>KONTAK FOOTER</label>
              <input type="text" v-model="footer.kontak" placeholder="Masukkan Kontak" class="form-control">
              <div v-if="validation.kontak" class="mt-2">
                <b-alert show variant="danger">{{ validation.kontak[0] }}</b-alert>
              </div>
            </div>
            <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i> UPDATE</button>
            <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i> RESET</button>
          </form>
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
      footer: {
        name: '',
        description: '',
        alamat: '',
        kontak: ''
      },
      validation: []
    }
  },
  mounted() {
    this.$axios.get(`/api/admin/footers/${this.$route.params.id}`)
      .then(response => {
        this.footer = { ...response.data.data, image: '' };
      })
      .catch(error => {
        console.error(error);
      });
  },
  methods: {
    async updateFooter() {
      let formData = new FormData();
      formData.append('name', this.footer.name);
      formData.append('description', this.footer.description);
      formData.append('alamat', this.footer.alamat);
      formData.append('kontak', this.footer.kontak);
      formData.append("_method", "PATCH");

      try {
        await this.$axios.post(`/api/admin/footers/${this.$route.params.id}`, formData);
        this.$swal.fire({
          title: 'BERHASIL!',
          text: "Data Berhasil Diupdate!",
          icon: 'success',
          showConfirmButton: false,
          timer: 2000
        });
        this.$router.push({ name: 'admin-footer' });
      } catch (error) {
        if (error.response && error.response.data.errors) {
          this.validation = error.response.data.errors;
        } else {
          console.error(error);
        }
      }
    }
  }
}
</script>

<style>
</style>
