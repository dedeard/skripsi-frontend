<template>
  <b-card header="Register" class="shadow-sm">
    <b-form @submit.prevent="submit">
      <b-alert variant="danger" :show="!!danger">{{ danger }}</b-alert>
      <b-form-group
        label="Your name:"
        label-for="name"
        :invalid-feedback="errors?.name"
      >
        <b-form-input
          id="name"
          v-model="form.name"
          type="text"
          placeholder="Enter name"
          :state="errors?.name ? false : null"
        />
      </b-form-group>
      <b-form-group
        label="Email address:"
        label-for="email"
        :invalid-feedback="errors?.email"
      >
        <b-form-input
          id="email"
          v-model="form.email"
          type="email"
          placeholder="Enter email"
          :state="errors?.email ? false : null"
        />
      </b-form-group>
      <b-form-group
        label="Password:"
        label-for="password"
        :invalid-feedback="errors?.password"
      >
        <b-form-input
          id="password"
          type="password"
          v-model="form.password"
          placeholder="Enter password"
          :state="errors?.password ? false : null"
        />
      </b-form-group>

      <b-button type="submit" variant="primary" block :disabled="loading">
        <b-spinner small v-if="loading" /> Daftar
      </b-button>
      <b-button to="/login" variant="light" block>Login</b-button>
    </b-form>
  </b-card>
</template>

<script>
export default {
  name: 'RegisterPage',
  layout: 'auth',
  middleware: ['guest'],
  head() {
    return {
      title: 'Register',
    }
  },
  data() {
    return {
      danger: '',
      errors: null,
      loading: false,
      form: {
        name: '',
        email: '',
        password: '',
      },
    }
  },
  methods: {
    async submit() {
      this.loading = true
      this.errors = null
      this.danger = ''
      try {
        await this.$axios.$post('/auth/register', this.form)
        this.$router.push('/login?register=1')
      } catch (e) {
        const { errors } = this.$errorResponse(e)
        this.errors = errors
        if (!errors) this.danger = this.$errorMessage(e)
        this.loading = false
      }
    },
  },
}
</script>
