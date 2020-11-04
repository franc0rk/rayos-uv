<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center q-mt-lg">
        <q-icon name="flare" color="accent" size="4em" />
        <h6 class="q-my-none">UV Detector</h6>
        <form @click.prevent="null">
          <div class="q-px-xl">
            <q-input name="usr" label="Usuario" class="q-my-md" color="accent" />
            <q-input name="psw" label="Contraseña" type="password" class="q-my-md" color="accent" />
            <q-btn type="submit" label="Ingresar" color="accent" class="q-my-md" />
            <q-btn type="button" flat label="Olvidaste tu contraseña?" color="accent" />
          </div>
        </form>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageLogin'
}
</script>
