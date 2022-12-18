<template>
  <div class="p-3">
    <div class="card shadow-sm">
      <div class="card-header d-flex">
        <div class="my-auto">Daftar Album</div>
      </div>
      <block-app-album-table :albums="albums" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  middleware: ['auth'],
  head() {
    return {
      title: 'Arsip Media',
    }
  },
  async asyncData({ $axios }) {
    const albums = await $axios.$get('/app/albums')
    return { albums }
  },
}
</script>
