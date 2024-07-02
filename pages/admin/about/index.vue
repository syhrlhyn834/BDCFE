<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'About'" />
      </div>
    </section>
    <div class="card-body">
      <form @submit.prevent="updateAbout">
        <div class="form-group">
          <label>ABOUT</label>
          <client-only placeholder="loading...">
            <ckeditor-nuxt v-model="abouts.description" :config="editorConfig" />
          </client-only>
          <div v-if="validation.description" class="mt-2">
            <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
          </div>
        </div>
        <button class="btn btn-info mr-1 btn-submit" type="submit">
          <i class="fa fa-paper-plane"></i> SIMPAN
        </button>
        <button class="btn btn-warning btn-reset" type="reset">
          <i class="fa fa-redo"></i> RESET
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import Title from '@/components/title.vue';

export default {
  layout: 'admin',

  components: {
    Title,
    'ckeditor-nuxt': () => {
      if (process.client) {
        return import('@blowstack/ckeditor-nuxt')
      }
    },
  },

  data() {
    return {
      //state about
      abouts: {
        description: ''
      },

      //state validation
      validation: [],

      //config CKEDITOR
      editorConfig: {
        removePlugins: ['Title'],
        simpleUpload: {
          uploadUrl: 'https://arlchoose.id/api/web/abouts/storeImage'
        }
      }
    }
  },

  mounted() {
    //fetching data about
    this.$axios.get(`/api/admin/abouts/1`)
      .then(response => {
        //assign response data to state "abouts.description"
        this.abouts.description = response.data.data.description
      })
  },

  methods: {
    async updateAbout() {
      // Reset validation errors
      this.validation = [];

      // Check if description is empty
      if (!this.abouts.description) {
        this.validation.description = ['Description cannot be empty'];
        return;
      }

      //define formData
      let formData = new FormData();
      formData.append('description', this.abouts.description);
      formData.append("_method", "PATCH");

      //sending data to server
      await this.$axios.post(`/api/admin/abouts/1`, formData)
        .then(() => {
          //sweet alert
          this.$swal.fire({
            title: 'BERHASIL!',
            text: "Data Berhasil Diupdate!",
            icon: 'success',
            showConfirmButton: false,
            timer: 2000
          });

          //redirect, if success store data
          this.$router.push({
            name: 'admin-about'
          });
        })
        .catch(error => {
          //assign error to state "validation"
          this.validation = error.response.data;
        });
    }
  }
}
</script>

<style>
  .ck-editor__editable {
    min-height: 200px;
  }
</style>
