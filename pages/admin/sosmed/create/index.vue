<template>
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Tambah Sosial Media'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> TAMBAH SOSMED</h3>
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
          <form @submit.prevent="storeSosmed">

            <div class="form-group">
              <label>LOGO SOSMED</label>
              <input type="text" v-model="sosmed.logo" placeholder="Masukkan Logo Sosmed" class="form-control">
              <div v-if="validation.logo" class="mt-2">
                <b-alert show variant="danger">{{ validation.logo[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>NAMA SOSMED</label>
              <input type="text" v-model="sosmed.name" placeholder="Masukkan Nama Sosmed" class="form-control">
              <div v-if="validation.name" class="mt-2">
                <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>URL SOSMED</label>
              <input type="text" v-model="sosmed.url" placeholder="Masukkan URL Sosmed" class="form-control">
              <div v-if="validation.url" class="mt-2">
                <b-alert show variant="danger">{{ validation.url[0] }}</b-alert>
              </div>
            </div>
            <div class="form-group">
              <label>USERNAME SOSMED</label>
              <input type="text" v-model="sosmed.username" placeholder="Masukkan Username Sosmed" class="form-control">
              <div v-if="validation.username" class="mt-2">
                <b-alert show variant="danger">{{ validation.username[0] }}</b-alert>
              </div>
            </div>

            <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
              SIMPAN</button>
            <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i>
              RESET</button>

          </form>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Title from '@/components/title.vue';

export default {
  //layout
  layout: 'admin',

  components: {
    Title
  },

  data() {
    return {
      //state sosmed
      sosmed: {
        logo: '',
        name: '',
        url: '',
        username: ''
      },
      //state validation
      validation: []
    }
  },

  methods: {
    async storeSosmed() {
      //define formData
      let formData = new FormData();

      formData.append('logo', this.sosmed.logo)
      formData.append('name', this.sosmed.name)
      formData.append('url', this.sosmed.url)
      formData.append('username', this.sosmed.username)

      //sending data to server
      try {
        await this.$axios.post('/api/admin/sosmeds', formData);
        //sweet alert
        this.$swal.fire({
          title: 'BERHASIL!',
          text: "Data Berhasil Disimpan!",
          icon: 'success',
          showConfirmButton: false,
          timer: 2000
        });

        //redirect, if success store data
        this.$router.push({
          name: 'admin-sosmed'
        });
      } catch (error) {
        //assign error to state "validation"
        this.validation = error.response.data;
      }
    }
  }
};
</script>

<style>
/* Tambahkan gaya CSS jika diperlukan */
</style>
