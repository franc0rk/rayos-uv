<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center q-mt-lg">
        <q-icon name="flare" color="accent" size="4em" />
        <h6 class="q-my-none">UV Detector</h6>
        <form @submit.prevent="login">
          <div class="q-px-xl">
            <q-input name="usr" label="Usuario" class="q-my-md" color="accent" v-model="credentials.email" />
            <q-input name="psw" label="Contraseña" type="password" class="q-my-md" color="accent" v-model="credentials.password" />
            <q-btn type="submit" label="Ingresar" color="accent" class="q-my-md" />
          </div>
        </form>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageLogin',
  data: () => ({
    credentials: {
      email: '',
      password: ''
    }
  }),
  methods: {
    login () {
      const URL = 'https://uv-api.herokuapp.com/auth/login'
      this.$axios.post(URL, this.credentials)
        .then(response => {
          if (response.data.token) {
            this.$q.notify({
              message: 'Bienvenido!',
              color: 'positive'
            })
            this.$router.push('/')
          }
        })
        .catch(err => {
          console.log(err)
          this.$q.notify({
            message: 'Credenciales no validas',
            color: 'negative'
          })
        })
    }
  }
}
</script>
