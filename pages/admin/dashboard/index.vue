<template>
  <!-- Content Wrapper. Contains page content -->
  <div class="content-wrapper">
    <section class="content-header">
      <div class="container-fluid">
        <Title :pageTitle="'Dashboard'" />
      </div>
    </section>

    <section class="content">
      <div class="row">
        <div class="col-md-3 col-sm-6 col-12">
          <div class="info-box">
            <span class="info-box-icon bg-info"><i class="fa fa-users"></i></span>
            <div class="info-box-content">
              <span class="info-box-text">USERS</span>
              <span>
                <hr></span>
              <span class="info-box-number">{{ users }}</span>
            </div>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="info-box">
            <span class="info-box-icon bg-primary"><i class="fa fa-users"></i></span>

            <div class="info-box-content">
              <span class="info-box-text">ANGGOTA</span>
              <span>
                <hr></span>
              <span class="info-box-number">{{ anggotas }}</span>
            </div>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="info-box">
            <span class="info-box-icon bg-success"><i class="fa fa-folder"></i></span>
            <div class="info-box-content">
              <span class="info-box-text">CATEGORIES</span>
              <span>
                <hr></span>
              <span class="info-box-number">{{ categories }}</span>
            </div>
          </div>
        </div>
        <div class="col-md-3 col-sm-6 col-12">
          <div class="info-box">
            <span class="info-box-icon bg-primary"><i class="fa fa-book-open"></i></span>

            <div class="info-box-content">
              <span class="info-box-text">BLOGS</span>
              <span>
                <hr></span>
              <span class="info-box-number">{{ blogs }}</span>
            </div>
          </div>
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


    async asyncData({ $axios }) {

      //fetching dashboard
      const dashboard = await $axios.$get('/api/admin/dashboard')

      return {
        'users': dashboard.data.users,
        'blogs': dashboard.data.blogs,
        'anggotas': dashboard.data.anggotas,
        'categories': dashboard.data.categories,
      }
    },

  }
</script>

<style>

</style>
