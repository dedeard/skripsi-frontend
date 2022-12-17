<template>
  <div>
    <b-button
      @click="$bvModal.show('modal-show-media-' + media.id)"
      variant="primary"
      size="sm"
      >Lihat</b-button
    >
    <b-modal
      :id="`modal-show-media-${media.id}`"
      :title="media.originalName"
      size="lg"
      no-close-on-backdrop
    >
      <div>
        <img
          class="d-block w-100"
          v-if="media.type === 'image'"
          :src="`${$config.baseApiUrl}/app/albums/media/${media.name}?auth=${token}`"
        />
        <video
          v-else
          class="d-block w-100"
          controls
          autoplay
          :src="`${$config.baseApiUrl}/app/albums/media/${media.name}?auth=${token}`"
        />
      </div>
      <template #modal-footer="{ cancel }">
        <b-button
          variant="primary"
          :download="media.originalName"
          :href="`${$config.baseApiUrl}/app/albums/media/${media.name}?auth=${token}&download=true`"
          >Download</b-button
        >
        <b-button variant="danger" @click="cancel()">Tutup</b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'ModalCreateAlbum',
  props: {
    media: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    token() {
      return this.$store.getters.token
    },
  },
}
</script>
