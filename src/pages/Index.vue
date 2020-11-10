<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center">
        <q-btn color="accent" size="4em" round icon="flare" @click="getUv" />
        <div class="text-subtitle1 q-my-xs">Presiona para obtener índice de rayos UV</div>
        <div>
          <h3 class="text-accent q-my-md">{{ base64ToStr(JSON.stringify(uvIndex)) }}</h3>
          <pre>{{ 'Dispositivo: ' + JSON.stringify(device.name).replace(/"/g, '') }}</pre>
          <pre class="cl"></pre>

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
    status: null,
    device: null,
    deviceData: null,
    uvIndex: null
  }),
  created () {
    if (window.bluetoothle) {
      // Bluetooth Init
      window.bluetoothle.initialize(result => {
        this.status = result
      }, {
        request: true,
        statusReceiver: true,
        restoreKey: 'bluetoothleplugin'
      })
      // Get Connected Device
      window.bluetoothle.retrieveConnected(data => {
        this.device = data[0]
        this.deviceConnection()
        document.address = data[0].address
      }, err => {
        console.log(err)
      }, { services: ['180D', '180F'] })
    }
  },
  methods: {
    getUv () {
      const UUID = this.deviceData.services[2].uuid.replace(/"/g, '')
      const address = this.device.address.replace(/"/g, '')
      const params = { address: address, service: UUID, characteristic: UUID }
      window.bluetoothle.unsubscribe(subscribeSuccess => {
        this.uvIndex = subscribeSuccess.value
      }, subscribeError => {
        this.uvIndex = subscribeError
      }, params)
      window.bluetoothle.subscribe(subscribeSuccess => {
        this.uvIndex = subscribeSuccess.value
      }, subscribeError => {
        this.uvIndex = subscribeError
      }, params)
    },
    deviceConnection () {
      const address = this.device.address.replace(/"/g, '')
      window.bluetoothle.discover(discoverSuccess => {
        this.deviceData = discoverSuccess
      }, discoverError => {
        this.deviceData = discoverError
      }, {
        address: address,
        clearCache: true
      })
    },
    base64ToStr (str) {
      return Buffer.from(str, 'base64').toString('ascii')
    }
  }
}
</script>
