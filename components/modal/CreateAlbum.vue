<template>
  <div>
    <b-button v-b-modal.modal-create-album variant="primary" size="sm"
      >Buat Album</b-button
    >
    <b-modal
      id="modal-create-album"
      title="Buat Album"
      size="lg"
      no-close-on-backdrop
    >
      <div>
        <b-alert variant="danger" :show="!!danger">{{ danger }}</b-alert>
        <b-form-group
          label="Nama Album:"
          label-for="name"
          :invalid-feedback="errors?.name"
        >
          <b-form-input
            id="name"
            v-model="form.name"
            placeholder="Nama"
            :state="errors?.name ? false : null"
          />
        </b-form-group>
        <b-form-group
          label="Bagian:"
          label-for="field"
          :invalid-feedback="errors?.fieldId"
        >
          <b-form-select
            id="field"
            v-model="form.fieldId"
            :options="[...fields.map((el) => ({ value: el.id, text: el.name }))]"
            :state="errors?.fieldId ? false : null"
          />
        </b-form-group>
        <b-form-group
          label="Deskripsi:"
          label-for="description"
          :invalid-feedback="errors?.description"
        >
          <b-form-textarea
            id="description"
            v-model="form.description"
            placeholder="Deskripsi"
            :state="errors?.description ? false : null"
          />
        </b-form-group>
      </div>
      <template #modal-footer="{ cancel }">
        <b-button variant="primary" :disabled="loading" @click="submit()"
          ><b-spinner small v-if="loading" /> Buat</b-button
        >
        <b-button variant="danger" @click="cancel()">Batal</b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'ModalCreateAlbum',
  props: {
    fields: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      loading: false,
      errors: null,
      danger: '',
      form: {
        name: '',
        fieldId: '',
        description: '',
      },
    }
  },
  methods: {
    async submit() {
      this.loading = true
      this.errors = null
      try {
        const album = await this.$axios.$post('/albums', this.form)
        this.$emit('onCreated', album)
        this.$bvModal.hide('modal-create-album')
        this.form = {
          name: '',
          description: '',
        }
        this.$bvToast.toast(`Berhasil membuat album baru.`, {
          variant: 'success',
          title: 'Sukses',
        })
      } catch (e) {
        const { errors } = this.$errorResponse(e)
        this.errors = errors
        if (!errors) this.danger = this.$errorMessage(e)
      }
      this.loading = false
    },
  },
}
</script>
