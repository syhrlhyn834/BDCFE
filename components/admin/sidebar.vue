<template>
  <!-- Sidebar Menu -->
  <nav class="mt-2">
    <ul class="nav nav-pills nav-sidebar flex-column" data-widget="treeview" role="menu" data-accordion="false">

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-dashboard'}" class="nav-link">
          <i class="nav-icon fas fa-tachometer-alt"></i>
          <p>
            Dashboard
          </p>
        </nuxt-link>
      </li>

      <li class="nav-header">
        <h6>CONTENT</h6>
      </li>


      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-category'}" class="nav-link">
          <i class="nav-icon fas fa-folder"></i>
          <p>
            Categories
          </p>
        </nuxt-link>
      </li>

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-blog'}" class="nav-link">
          <i class="nav-icon fas fa-book-open"></i>
          <p>
            Blogs
          </p>
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-anggota'}" class="nav-link">
          <i class="nav-icon fas fa-book-open"></i>
          <p>
            Anggota
          </p>
        </nuxt-link>
      </li>

      <li class="nav-header">
        <h6>SETTINGS</h6>
      </li>

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-header'}" class="nav-link">
          <i class="nav-icon fas fa-clone"></i>
          <p>
            Header
          </p>
        </nuxt-link>
      </li>

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-footer'}" class="nav-link">
          <i class="nav-icon fas fa-clone"></i>
          <p>
            Footer
          </p>
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-about'}" class="nav-link">
          <i class="nav-icon fas fa-clone"></i>
          <p>
            About
          </p>
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-slider'}" class="nav-link">
          <i class="nav-icon fas fa-image"></i>
          <p>
            Sliders
          </p>
        </nuxt-link>
      </li>

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-sosmed'}" class="nav-link">
          <i class="nav-icon fas fa-image"></i>
          <p>
            Sosmed
          </p>
        </nuxt-link>
      </li>

      <li class="nav-item">
        <nuxt-link :to="{name: 'admin-user'}" class="nav-link">
          <i class="nav-icon fas fa-users"></i>
          <p>
            Users
          </p>
        </nuxt-link>
      </li>

    </ul>
  </nav>
  <!-- /.sidebar-menu -->
</template>

<script>
  export default {

  }
</script>

<style scoped>
  a.nuxt-link-active {
    background-color: rgba(255, 255, 255, .1);
  }
</style>
