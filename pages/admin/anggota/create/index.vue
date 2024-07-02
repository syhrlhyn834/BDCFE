<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Tambah Anggota'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> TAMBAH Anggota</h3>
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
          <form @submit.prevent="storeAnggota">
            <div class="form-group">
              <label>GAMBAR Anggota</label>
              <input type="file" @change="handleFileChange" class="form-control">
              <div v-if="validation.image" class="mt-2">
                <b-alert show variant="danger">{{ validation.image[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>NAMA ANGGOTA</label>
              <input type="text" v-model="anggota.name" placeholder="Masukkan Nama Anggota" class="form-control">
              <div v-if="validation.name" class="mt-2">
                <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>JABATAN</label>
              <input type="text" v-model="anggota.posisi" placeholder="Masukkan Posisi Anggota" class="form-control">
              <div v-if="validation.posisi" class="mt-2">
                <b-alert show variant="danger">{{ validation.posisi[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>UMUR</label>
              <input type="text" v-model="anggota.umur" placeholder="Masukkan Umur Anggota (Angka)" class="form-control">
              <div v-if="validation.umur" class="mt-2">
                <b-alert show variant="danger">{{ validation.umur[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>ALAMAT</label>
              <input type="text" v-model="anggota.alamat" placeholder="Masukkan Alamat Anggota" class="form-control">
              <div v-if="validation.alamat" class="mt-2">
                <b-alert show variant="danger">{{ validation.alamat[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>DESCRIPTION</label>
              <textarea v-model="anggota.description" class="form-control" rows="3" placeholder="Masukkan Deskripsi Singkat"></textarea>
              <div v-if="validation.description" class="mt-2">
                <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
              </div>
            </div>

            <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i> SIMPAN</button>
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

  components: {
    Title
  },

  data() {
    return {
      anggota: {
        image: '',
        name: '',
        posisi: '',
        umur: '',
        alamat: '',
        description: ''
      },
      validation: []
    };
  },

  methods: {
    handleFileChange(e) {
      let image = this.anggota.image = e.target.files[0];
      if (!image.type.match('image.*')) {
        e.target.value = '';
        this.anggota.image = null;
        this.$swal.fire({
          title: 'OOPS!',
          text: "Format File Tidak Didukung!",
          icon: 'error',
          showConfirmButton: false,
          timer: 2000
        });
      }
    },

    async storeAnggota() {
      let formData = new FormData();
      formData.append('image', this.anggota.image);
      formData.append('name', this.anggota.name);
      formData.append('posisi', this.anggota.posisi);
      formData.append('umur', this.anggota.umur);
      formData.append('alamat', this.anggota.alamat);
      formData.append('description', this.anggota.description);

      try {
        await this.$axios.post('/api/admin/anggotas', formData);
        this.$swal.fire({
          title: 'BERHASIL!',
          text: "Data Berhasil Disimpan!",
          icon: 'success',
          showConfirmButton: false,
          timer: 2000
        });
        this.$router.push({ name: 'admin-anggota' });
      } catch (error) {
        this.validation = error.response.data;
      }
    }
  }
};
</script>

<style scoped>
</style>
