<template>
  <main class="bg-secondary">
    <header class="bg-white shadow-sm">
      <div class="container py-3">
        <nuxt-link to="/" class="text-center d-block text-dark">
          <img
            src="/logo.png"
            width="50"
            class="d-block mx-auto mb-2"
            alt="Logo dinas kesehatan prov. sulsel"
          />
          <span class="font-weight-bold h5 d-block">ARSIP MEDIA</span>
        </nuxt-link>
      </div>
    </header>
    <nuxt />
  </main>
</template>

<script>
export default {
  name: 'ShareLayout',
}
</script>

<style scoped>
main {
  min-height: 100vh;
}
</style>
