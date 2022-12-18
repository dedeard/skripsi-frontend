<template>
  <div>
    <b-button
      @click="$bvModal.show('modal-user-albums-' + uid)"
      variant="primary"
      size="sm"
      >Album</b-button
    >
    <b-modal
      :id="'modal-user-albums-' + uid"
      title="User Albums"
      size="lg"
      body-class="p-0"
      no-close-on-backdrop
    >
      <div>
        <b-table
          class="m-0"
          striped
          :fields="[
            { key: 'id', label: 'ID' },
            { key: 'name', label: 'Nama' },
            { key: 'action', label: 'Aksi', class: 'text-center width-80' },
          ]"
          :items="albums.filter((el) => !userAlbums.includes(el.id))"
        >
          <template #cell(action)="{ item }">
            <b-button variant="primary" size="sm" @click="give(item.id)">
              GIVE
            </b-button>
          </template>
        </b-table>
        <b-table
          class="m-0"
          striped
          :fields="[
            { key: 'id', label: 'ID' },
            { key: 'name', label: 'Nama' },
            { key: 'action', label: 'Aksi', class: 'text-center width-80' },
          ]"
          :items="albums.filter((el) => userAlbums.includes(el.id))"
        >
          <template #cell(action)="{ item }">
            <b-button variant="danger" size="sm" @click="remove(item.id)">
              REMOVE
            </b-button>
          </template>
        </b-table>
      </div>
      <template #modal-footer="{ cancel }">
        <b-button variant="danger" @click="cancel()">Tutup</b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'ModalUserAlbums',
  props: {
    uid: {
      type: Number,
      default: 0,
    },
    userAlbums: {
      type: Array,
      default: () => [],
    },
    albums: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      error: '',
    }
  },
  methods: {
    async give(id) {
      try {
        await this.$axios.$post(`/users/${this.uid}/${id}`)
        this.$emit('onUpdated', { id: this.uid, albums: [...this.albums, id] })
      } catch (e) {
        this.$sw('Give failed', this.$errorMessage(e))
      }
    },
    async remove(id) {
      try {
        await this.$axios.$delete(`/users/${this.uid}/${id}`)
        this.$emit('onUpdated', {
          id: this.uid,
          albums: [...this.albums].filter((el) => el.id !== id),
        })
      } catch (e) {
        this.$sw('Remove failed', this.$errorMessage(e))
      }
    },
  },
}
</script>
