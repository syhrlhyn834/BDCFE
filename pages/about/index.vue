<template>
  <div>
    <Title :pageTitle="'About'" />

    <b-container class="mt-5 mb-5">
      <b-row>

        <b-col md="12" sm="12">
          <b-card no-body class="border-0 rounded-lg shadow-md mb-3" v-for="about in abouts" :key="about.id">
            <b-row class="g-0">
              <b-col md="8">
                <b-card-body>
                  <b-card-text> <div v-html="about.description"></div></b-card-text>
                  </b-card-body></b-col></b-row></b-card>


        </b-col>
      </b-row>
    </b-container>


  </div>
</template>

<script>
  import Title from '@/components/title.vue';

  export default {

    async asyncData({ $axios }) {

      //fetching abouts
      const abouts = await $axios.$get('/api/web/abouts')


      return {
        'abouts': abouts.data,

      }
    }

  }
</script>

<style>
</style>
