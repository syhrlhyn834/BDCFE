<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Edit Anggota'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> EDIT ANGGOTA</h3>
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
          <form @submit.prevent="updateAnggota">

            <div class="form-group">
              <label>GAMBAR ANGGOTA</label>
              <input type="file" @change="handleFileChange" class="form-control">
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

            <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
              UPDATE</button>
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
      'ckeditor-nuxt': () => {
        if (process.client) {
          return import('@blowstack/ckeditor-nuxt')
        }
      },
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
    mounted() {

      //fetching data anggota
      this.$axios.get(`/api/admin/anggotas/${this.$route.params.id}`)

        .then(response => {

          //assing response data to state "anggota.name"
          this.anggota.name = response.data.data.name

          //assing response data to state "anggota.posisi"
          this.anggota.posisi = response.data.data.posisi

          //assing response data to state "anggota.umur"
          this.anggota.umur = response.data.data.umur

          //assing response data to state "anggota.description"
          this.anggota.description = response.data.data.description

          //assing response data to state "anggota.alamat"
          this.anggota.alamat = response.data.data.alamat
        })

    },

    methods: {

      handleFileChange(e) {

        //get image
        let image = this.anggota.image = e.target.files[0]

        //check fileType
        if (!image.type.match('image.*')) {

          //if fileType not allowed, then clear value and set null
          e.target.value = ''

          this.anggota.image = null

          //show sweet alert
          this.$swal.fire({
            title: 'OOPS!',
            text: "Format File Tidak Didukung!",
            icon: 'error',
            showConfirmButton: false,
            timer: 2000
          })
        }

      },

      async updateAnggota() {


        //define formData
        let formData = new FormData();

        formData.append('image', this.anggota.image);
      formData.append('name', this.anggota.name);
      formData.append('posisi', this.anggota.posisi);
      formData.append('umur', this.anggota.umur);
      formData.append('alamat', this.anggota.alamat);
      formData.append('description', this.anggota.description);
        formData.append("_method", "PATCH")

        //sending data to server
        await this.$axios.post(`/api/admin/anggotas/${this.$route.params.id}`, formData)
          .then(() => {

            //sweet alert
            this.$swal.fire({
              title: 'BERHASIL!',
              text: "Data Berhasil Diupdate!",
              icon: 'success',
              showConfirmButton: false,
              timer: 2000
            })

            //redirect, if success store data
            this.$router.push({
              name: 'admin-anggota'
            })

          })
          .catch(error => {

            //assign error to state "validation"
            this.validation = error.response.data
          })

      }
    }

  }
</script>

<style>
</style>
