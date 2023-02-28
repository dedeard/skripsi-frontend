<template>
  <main class="bg-secondary">
    <header class="d-flex position-fixed w-100 bg-white shadow-sm px-3">
      <div class="d-flex w-100 my-auto">
        <nuxt-link to="/" class="d-flex text-dark text-decoration-none">
          <div class="my-auto pr-2">
            <img src="/logo.png" height="30px" />
          </div>
          <div class="my-auto">
            <span class="font-weight-bold">ARSIP MEDIA</span>
          </div>
        </nuxt-link>
        <div class="ml-auto">
          <b-button
            size="sm"
            variant="danger"
            class="font-weight-bold"
            @click="logout"
            >LOG OUT</b-button
          >
        </div>
      </div>
    </header>
    <aside class="bg-white shadow-sm position-fixed">
      <div class="w-full aside-wrapper py-3">
        <div class="px-3 text-center">
          <div class="p-3 border rounded bg-secondary">
            <div class="font-weight-bold lead m-0 text-capitalize">
              {{ $store.state.user.name }}
            </div>
            <div class="text-muted small">{{ $store.state.user.email }}</div>
            <b-badge variant="primary">{{
              $store.getters.role?.name || ''
            }}</b-badge>
          </div>
        </div>
        <div class="p-3">
          <b-nav vertical pills>
            <b-nav-item
              to="/"
              exact-active-class="active"
              class="font-weight-bold pr-0"
              >Home</b-nav-item
            >
            <b-nav-item
              v-if="$store.getters.role"
              to="/dashboard"
              active-class="active"
              class="font-weight-bold pr-0"
              >Dashboard</b-nav-item
            >
            <b-nav-item
              v-if="$can('Read Role|Create Role|Update Role|Delete Role')"
              to="/roles"
              active-class="active"
              class="font-weight-bold pr-0"
              >Peran</b-nav-item
            >
            <b-nav-item
              v-if="$can('Read User|Create User|Update User|Delete User')"
              to="/users"
              active-class="active"
              class="font-weight-bold pr-0"
              >Users</b-nav-item
            >
            <b-nav-item
              v-if="$can('Read Album|Create Album|Update Album|Delete Album')"
              to="/albums"
              active-class="active"
              class="font-weight-bold pr-0"
              >Album</b-nav-item
            >
          </b-nav>
        </div>
      </div>
    </aside>
    <nuxt />
  </main>
</template>

<script>
export default {
  name: 'DefaultLayout',
  methods: {
    async logout() {
      const { isConfirmed } = await this.$swal({
        title: 'Anda yakin ingin keluar dari akun anda?',
        icon: 'warning',
        cancelButtonColor: '#007bff',
        confirmButtonColor: '#dc3545',
        cancelButtonText: 'Batal',
        showCancelButton: true,
        confirmButtonText: 'Ya, Logout',
      })
      if (!isConfirmed) return true
      this.$store.commit('removeToken')
      window.location.reload()
    },
  },
}
</script>

<style scoped>
main {
  min-height: 100vh;
  padding-left: 260px;
  padding-top: 60px;
}
header {
  z-index: 999;
  height: 60px;
  top: 0;
  left: 0;
}
aside {
  height: 100vh;
  width: 260px;
  padding-top: 60px;
  z-index: 800;
  top: 0;
  left: 0;
}
.aside-wrapper {
  height: calc(100vh - 60px);
  overflow-y: scroll;
}
</style>
