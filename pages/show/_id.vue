<template>
  <b-container class="py-3">
    <b-card no-body class="shadow-sm">
      <div class="card-body">
        <h3 class="text-capitalize">{{ album.name }}</h3>
        <p class="m-0 lead">
          <span v-html="description.replaceAll('\n', '<br />')"></span>
          <a
            href="#"
            v-if="Number(album.description?.length) > 300"
            @click.prevent="truncate = !truncate"
            variant="primary"
          >
            Show {{ truncate ? 'more' : 'less' }}
          </a>
        </p>
      </div>
      <b-table
        :per-page="perPage"
        :current-page="currentPage"
        striped
        :fields="[
          { key: 'type', label: 'Jenis' },
          { key: 'size', label: 'Ukuran' },
          { key: 'originalName', label: 'Nama' },
          { key: 'action', label: 'Aksi', class: 'text-center width-80' },
        ]"
        :items="album.media"
      >
        <template #cell(size)="{ item }">
          {{ prettyBytes(item.size) }}
        </template>
        <template #cell(action)="{ item }">
          <modal-show-album-media
            class="d-inline-block"
            :media="item"
            :aid="album.id"
          />
        </template>
      </b-table>
      <div class="p-3">
        <b-pagination
          v-model="currentPage"
          :total-rows="album.media.length"
          :per-page="perPage"
          align="center"
        ></b-pagination>
      </div>
    </b-card>
  </b-container>
</template>

<script>
import prettyBytes from 'pretty-bytes'
export default {
  name: 'ShowAlbumPage',
  layout: 'share',
  head() {
    return {
      title: this.album.name,
    }
  },
  async asyncData({ $axios, params }) {
    const album = await $axios.$get('/app/albums/' + params.id)
    return {
      album,
      id: params.id,
      perPage: 10,
      currentPage: 1,
      truncate: true,
    }
  },
  computed: {
    description() {
      if (this.truncate && Number(this.album.description?.length) > 300) {
        return this.album.description.substring(0, 300) + '...'
      }
      return this.album.description
    },
  },
  methods: {
    prettyBytes(b) {
      return prettyBytes(b)
    },
  },
}
</script>
