<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Edit Header'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> EDIT HEADERS</h3>
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
          <form @submit.prevent="updateHeader">
            <div class="form-group">
              <label>LOGO</label>
              <input type="file" @change="handleFileChange" class="form-control">
            </div>
            <div class="form-group">
              <label>TITLE WEBSITE</label>
              <input type="text" v-model="header.title" placeholder="Masukkan Nama Website" class="form-control">
              <div v-if="validation.title" class="mt-2">
                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>NAMA WEBSITE</label>
              <input type="text" v-model="header.name" placeholder="Masukkan Nama Website" class="form-control">
              <div v-if="validation.name" class="mt-2">
                <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>DESKRIPSI WEBSITE</label>
              <input type="text" v-model="header.description" placeholder="Masukkan Description" class="form-control">
              <div v-if="validation.description" class="mt-2">
                <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
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
import Title from '@/components/Title.vue';
export default {
  layout: 'admin',
  data() {
    return {
      header: {
        image: '',
        title: '',
        name: '',
        description: ''
      },
      validation: []
    }
  },
  mounted() {
    this.$axios.get(`/api/admin/headers/${this.$route.params.id}`)
      .then(response => {
        this.header = { ...response.data.data, image: '' };
      })
      .catch(error => {
        console.error(error);
      });
  },
  methods: {
    handleFileChange(e) {
      let image = e.target.files[0];
      if (image && image.type.match('image.*')) {
        this.header.image = image;
      } else {
        e.target.value = '';
        this.header.image = null;
        this.$swal.fire({
          title: 'OOPS!',
          text: "Format File Tidak Didukung!",
          icon: 'error',
          showConfirmButton: false,
          timer: 2000
        });
      }
    },
    async updateHeader() {
      let formData = new FormData();
      if (this.header.image) {
        formData.append('image', this.header.image);
      }
      formData.append('title', this.header.title);
      formData.append('name', this.header.name);
      formData.append('description', this.header.description);
      formData.append("_method", "PATCH");

      try {
        await this.$axios.post(`/api/admin/headers/${this.$route.params.id}`, formData);
        this.$swal.fire({
          title: 'BERHASIL!',
          text: "Data Berhasil Diupdate!",
          icon: 'success',
          showConfirmButton: false,
          timer: 2000
        });
        this.$router.push({ name: 'admin-header' });
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
