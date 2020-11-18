<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center">
        <q-btn color="accent" size="4em" round icon="flare" @click="uvStream" />
        <div class="text-subtitle1 q-my-xs">Presiona para obtener índice de rayos UV</div>
        <div>
          <h3 class="text-accent q-my-md">{{ base64ToStr(JSON.stringify(uvIndex)) }}</h3>
          <pre>{{ 'Dispositivo: ' + JSON.stringify(device.name).replace(/"/g, '') }}</pre>
          <pre>{{ 'Dispositivo: ' + JSON.stringify(deviceData).replace(/"/g, '') }}</pre>
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
    uvIndex: null,
    connectionStatus: null
  }),
  created () {
    // logout if not authenticated user
    if (!localStorage.getItem('userId')) {
      this.$router.push('/login')
    }

    if (window.bluetoothle) {
      // Bluetooth Init
      window.bluetoothle.initialize(resp => {
        this.status = resp
      }, {
        request: true,
        statusReceiver: true,
        restoreKey: 'bluetoothleplugin'
      })
      // Get Connected Device
      window.bluetoothle.retrieveConnected(data => {
        this.device = data[0]
      }, err => {
        console.log(err)
      }, { services: ['180D', '180F'] })
      // Connect Device
      window.bluetoothle.connect(connectSuccess => {
        this.connectionStatus = connectSuccess
      }, connectError => {
        this.connectionStatus = connectError
      }, { address: 'F0:08:D1:D8:22:C2' })
      // Get Connected Device
      window.bluetoothle.discover(discoverSuccess => {
        this.deviceData = discoverSuccess
        this.uvStream()
      }, discoverError => {
        this.deviceData = discoverError
      }, { address: 'F0:08:D1:D8:22:C2', clearCache: true })
    }
  },
  unmounted () {
    const UUID = this.deviceData.services[2].uuid.replace(/"/g, '')
    const address = this.device.address.replace(/"/g, '')
    const params = { address: address, service: UUID, characteristic: UUID }
    window.bluetoothle.unsubscribe(subscribeSuccess => {
      this.uvIndex = subscribeSuccess.value
    }, subscribeError => {
      this.uvIndex = subscribeError
    }, params)
    // Disconnect Device
    window.bluetoothle.disconnect(disconnectSuccess => {
      this.connectionStatus = disconnectSuccess
    }, disconnectError => {
      this.connectionStatus = disconnectError
    }, { address: 'F0:08:D1:D8:22:C2' })
  },
  methods: {
    uvStream () {
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
        this.saveQuery(this.base64ToStr(JSON.stringify(this.uvIndex)))
      }, subscribeError => {
        this.uvIndex = subscribeError
      }, params)
    },
    saveQuery (value) {
      const userId = localStorage.getItem('userId')
      let level = 'bajo'

      if (value >= 5 && level <= 7) {
        level = 'medio'
      }

      if (value > 7) {
        level = 'alto'
      }

      this.$axios.post('https://uv-api.herokuapp.com/consultas', {
        uv_index: value,
        role: level,
        date: new Date(),
        user_id: userId
      }).then(data => {
        console.log(data)
      }).catch(err => {
        console.log(err)
      })
    },
    base64ToStr (str) {
      return Buffer.from(str, 'base64').toString('ascii')
    }
  }
}
</script>
