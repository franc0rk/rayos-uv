<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center">
        <q-btn color="accent" size="4em" round icon="flare" />
        <div class="text-subtitle1 q-my-xs">Presiona para obtener índice de rayos UV</div>
        <div>
          <h1 class="text-accent q-my-md">{{ 'Índice' }}</h1>
          <!--semaphore-->
          <div>
            <q-icon name="fiber_manual_record" color="red" size="4em" style="opacity: 0.4" />
            <q-icon name="fiber_manual_record" color="amber" size="4em" style="opacity: 0.4" />
            <q-icon name="fiber_manual_record" color="green" size="4em" />
            <p class="q-my-md text-green">Los rayos UV no afectan demasiado en tu zona</p>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>
