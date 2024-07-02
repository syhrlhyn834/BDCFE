<template>
  <div class="card card-outline card-info">
    <div class="card-header text-center">
      <nuxt-link to="/" class="h1 font-weight-bold text-dark">Admin Login</nuxt-link>
      <div><i>by Syahril Haryono</i></div>
    </div>
    <div class="card-body">
      <div v-if="validation.message" class="mt-2">
        <b-alert show variant="danger">{{ validation.message }}</b-alert>
      </div>
      <form @submit.prevent="login">
        <div class="form-group">
          <label class="font-weight-bold text-uppercase">Email/Nomor Telepon</label>
          <input type="text" v-model="user.login" :class="{ 'is-invalid': validation.login }" class="form-control"
            placeholder="Masukkan Email atau Nomor Telepon">
        </div>
        <div v-if="validation.login" class="mt-2">
          <b-alert show variant="danger">{{ validation.login[0] }}</b-alert>
        </div>

        <div class="form-group">
          <label class="font-weight-bold text-uppercase">Password</label>
          <input type="password" v-model="user.password" :class="{ 'is-invalid': validation.password }"
            class="form-control" placeholder="Masukkan Password">
        </div>
        <div v-if="validation.password" class="mt-2">
          <b-alert show variant="danger">{{ validation.password[0] }}</b-alert>
        </div>

        <button type="submit" class="btn btn-info btn-block">LOGIN</button>
      </form>
    </div>
    <!-- /.card-body -->
  </div>
</template>

<script>
  export default {
    //layout
    layout: 'auth',

    //meta
    head() {
      return {
        title: 'Login - ByteDevCode Admin',
      }
    },

    data() {
      return {
        //state user
        user: {
          login: '',
          password: '',
        },
        //validation
        validation: []
      }
    },

    methods: {
      async login() {
        await this.$auth.loginWith('local', {
            data: {
              login: this.user.login,
              password: this.user.password
            }
          })
          .then(() => {
            //redirect
            this.$router.push({
              name: 'admin-dashboard'
            })
          })
          .catch(error => {
            //assign validation
            this.validation = error.response.data
          })
      }
    }
  }
</script>

<style>
/* Add any relevant styles here */
</style>
