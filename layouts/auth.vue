<template>
  <main class="auth-main d-flex">
    <div class="container py-5 my-auto">
      <div class="row">
        <div class="col-md-8 col-lg-6 mx-auto py-3 bg-white rounded shadow-lg">
          <div class="py-3">
            <div class="text-center">
              <img
                src="/logo.png"
                width="80"
                class="d-block mx-auto mb-2"
                alt="Logo dinas kesehatan prov. sulsel"
              />
              <span class="font-weight-bold h4 d-block mb-0"
                >LOGIN ARSIP MEDIA</span
              >
              <span class="font-weight-bold d-block m-0"
                >DINAS KESEHATAN PROVINSI SULAWESI SELATAN</span
              >
            </div>
          </div>
          <nuxt />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'AuthLayout',
}
</script>

<style scoped>
.auth-main {
  background-image: url(/dinkes.jpg);
  background-size: cover;
  margin: 0;
  min-height: 100vh;
}
</style>
