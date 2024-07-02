<template>
  <b-container class="mt-5 mb-5">
    <b-row>
      <!-- Detail Anggota -->
      <b-col md="8">
        <b-card>
          <b-card-img :src="anggota.image" alt="Image" class="rounded"></b-card-img>
          <b-card-body>
            <b-row>
              <b-col md="4"><strong>Nama Lengkap</strong></b-col>
              <b-col md="8">{{ anggota.name }}</b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col md="4"><strong>Jabatan</strong></b-col>
              <b-col md="8">{{ anggota.posisi }}</b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col md="4"><strong>Umur</strong></b-col>
              <b-col md="8">{{ anggota.umur }} Tahun</b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col md="4"><strong>Alamat</strong></b-col>
              <b-col md="8">{{ anggota.alamat }}</b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col md="4"><strong>Deskripsi</strong></b-col>
              <b-col md="8">{{ anggota.description }}</b-col>
            </b-row>
          </b-card-body>
        </b-card>
      </b-col>
      <!-- Anggota Lainnya -->
      <b-col md="4">
        <b-card>
          <b-card-header>
            <h4>Anggota Lainnya</h4>
          </b-card-header>
          <b-list-group flush>
            <b-list-group-item
              v-for="item in anggotaLainnya"
              :key="item.id"
              class="d-flex align-items-center"
            >
              <b-img
                :src="item.image"
                fluid
                class="me-3 rounded"
                style="width: 50px; height: 50px;"
              ></b-img>
              <div class="ms-3">
                <nuxt-link :to="{ name: 'anggota-slug', params: { slug: item.slug }}">
                  <h6 class="mb-1">{{ item.name }}</h6>
                  <p class="text-black">{{ item.posisi }}</p>
                </nuxt-link>
              </div>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  async asyncData({ params, $axios }) {
    // Fetch specific anggota data by slug
    const response = await $axios.$get(`/api/web/anggotas/${params.slug}`);
    const anggota = response.data;

    // Fetch other anggota data for the sidebar
    const allAnggotasResponse = await $axios.$get('/api/web/anggotas');
    const anggotaLainnya = allAnggotasResponse.data.data.filter(
      item => item.slug !== params.slug
    );

    return {
      anggota,
      anggotaLainnya
    };
  }
}
</script>

<style scoped>
.text-black {
  color: black;
}

h2 {
  color: #000000;
}

.b-list-group-item .nuxt-link {
  display: block;
}

.b-list-group-item .nuxt-link h6 {
  margin-bottom: 0.25rem; /* Add margin between name and position */
}

.b-list-group-item .nuxt-link p {
  margin-bottom: 0; /* Ensure no extra margin below position */
}

.ms-3 {
  margin-left: 1rem; /* Add margin-left to provide spacing */
}

.rounded {
  border-radius: 8px; /* Adjust this value as needed */
}
</style>
