<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center">
        <q-btn color="accent" size="4em" round icon="flare" @click="getUv" />
        <div class="text-subtitle1 q-my-xs">Presiona para obtener índice de rayos UV</div>
        <div>
          <h1 class="text-accent q-my-md">{{ 'Índice' }}</h1>
          <pre>{{ 'Resultado suscripcion: ' + JSON.stringify(result) }}</pre>
          <pre>{{ 'Dispositivos: ' + dispositivos }}</pre>

          <!--semaphore-->
          <div>
            <q-icon name="fiber_manual_record" color="red" size="4em" style="opacity: 0.4" />
            <q-icon name="fiber_manual_record" color="amber" size="4em" style="opacity: 0.4" />
            <q-icon name="fiber_manual_record" color="green" size="4em" />
            <p class="q-my-md text-green">Los rayos UV no afectan demasiado en tu zona</p>
            <p class="q-my-md"><q-icon name="info" /> Más información</p>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data: () => ({
    devices: [],
    result: null,
    dispositivos: null
  }),
  created () {
    if (window.bluetoothle) {
      window.bluetoothle.initialize(result => {
        console.log('result', result)
      })

      window.bluetoothle.enable(data => {
        console.log('data', data)
      }, err => {
        console.log(err)
      })

      window.bluetoothle.connect(data => {
        console.log(data)
      }, err => {
        console.log(err)
      }, { address: '283f04ce-fd1d-4db3-a74a-636b51dc4809' })

      // bluetoothle.addService(success, error, params);
    }
  },
  methods: {
    getUv () {
      window.bluetoothle.retrieveConnected(data => {
        console.log(data)
        this.dispositivos = data
      }, err => {
        console.log(err)
      }, { services: ['180D', '180F'] })

      window.bluetoothle.subscribe(data => {
        console.log(data)
        this.result = data
      }, err => {
        console.log(err)
        this.result = err
      }, {
        address: '283f04ce-fd1d-4db3-a74a-636b51dc4809',
        service: '283f04ce-fd1d-4db3-a74a-636b51dc4809'
      })
      // window.bluetoothle.read(readSuccess, readError, {
      //   "address": "ECC037FD-72AE-AFC5-9213-CA785B3B5C63",
      //   "service": "180d",
      //   "characteristic": "2a38"
      // });
    }
  }
}
</script>
