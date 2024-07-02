<template>
  <div class="content-wrapper mb-5">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Tambah Blog'" />
      </div>
    </section>

    <section class="content">
      <div class="card card-outline card-info">
        <div class="card-header">
          <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> TAMBAH BLOG</h3>
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
          <form @submit.prevent="storeBlog">

            <div class="form-group">
              <label>GAMBAR BLOG</label>
              <input type="file" @change="handleFileChange" class="form-control">
              <div v-if="validation.image" class="mt-2">
                <b-alert show variant="danger">{{ validation.image[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>JUDUL BLOG</label>
              <input type="text" v-model="blog.title" placeholder="Masukkan Judul Blog" class="form-control">
              <div v-if="validation.title" class="mt-2">
                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>CATEGORY</label>
              <multiselect v-model="blog.category_id" :options="categories" label="name" track-by="id" :searchable="true"></multiselect>
              <div v-if="validation.category_id" class="mt-2">
                <b-alert show variant="danger">{{ validation.category_id[0] }}</b-alert>
              </div>
            </div>

            <div class="form-group">
              <label>KONTEN BLOG</label>
              <client-only placeholder="loading...">
                <ckeditor-nuxt v-model="blog.content" :config="editorConfig" />
              </client-only>
              <div v-if="validation.content" class="mt-2">
                <b-alert show variant="danger">{{ validation.content[0] }}</b-alert>
              </div>
            </div>


            <div class="form-group">
              <label>DESCRIPTION</label>
              <textarea v-model="blog.description" class="form-control" rows="3"
                placeholder="Masukkan Deskripsi Singkat"></textarea>
              <div v-if="validation.description" class="mt-2">
                <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
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
      'ckeditor-nuxt': () => {
        if (process.client) {
          return import('@blowstack/ckeditor-nuxt')
        }
      },
    },

    data() {
      return {
        //state blog
        blog: {
          image: '',
          title: '',
          category_id: '',
          content: '',
          description: ''
        },

        //state categories
        categories: [],


        //state validation
        validation: [],

        //config CKEDITOR
        editorConfig: {
          removePlugins: ['Title'],
          simpleUpload: {
            uploadUrl: 'https://api.arlchoose.id/api/web/blogs/storeImage'
          }
        }
      }
    },

    mounted() {

      //fetching data categories
      this.$axios.get('/api/admin/categories')

        .then(response => {

          //assing response data to state "categories"
          this.categories = response.data.data.data
        })

      //fetching data tags
      this.$axios.get('/api/admin/tags')

        .then(response => {

          //assing response data to state "tags"
          this.tags = response.data.data.data
        })

    },

    methods: {

      handleFileChange(e) {

        //get image
        let image = this.blog.image = e.target.files[0]

        //check fileType
        if (!image.type.match('image.*')) {

          //if fileType not allowed, then clear value and set null
          e.target.value = ''

          this.blog.image = null

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

      async storeBlog() {


        //define formData
        let formData = new FormData();

        formData.append('image', this.blog.image)
        formData.append('title', this.blog.title)
        formData.append('category_id', this.blog.category_id ? this.blog.category_id.id : '')
        formData.append('content', this.blog.content)
        formData.append('description', this.blog.description)

        //sending data to server
        await this.$axios.post('/api/admin/blogs', formData)
          .then(() => {

            //sweet alert
            this.$swal.fire({
              title: 'BERHASIL!',
              text: "Data Berhasil Disimpan!",
              icon: 'success',
              showConfirmButton: false,
              timer: 2000
            })

            //redirect, if success store data
            this.$router.push({
              name: 'admin-blog'
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
  .ck-editor__editable {
    min-height: 200px;
  }
</style>
