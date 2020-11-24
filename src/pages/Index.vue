<template>
  <q-page class="row">
    <div class="col q-my-lg">
      <div class="text-center">
        <q-btn color="accent" size="4em" round icon="flare" @click="uvStream" />
        <div class="text-subtitle1 q-my-xs">Presiona para obtener índice de rayos UV</div>
        <div>
          <h3 class="text-accent q-my-md">{{ base64ToStr(JSON.stringify(uvIndex)) }}</h3>
          <pre v-if="device">{{ 'Dispositivo: ' + JSON.stringify(device.name).replace(/"/g, '') }}</pre>
          <pre>{{ 'Dispositivo: ' + JSON.stringify(deviceData).replace(/"/g, '') }}</pre>
          <pre class="cl"></pre>

          <!--semaphore-->
          <div>
            <q-icon name="fiber_manual_record" color="red" size="4em" :style="level === 'alto' ? 'opacity: 1' : 'opacity: 0.4'" />
            <q-icon name="fiber_manual_record" color="amber" size="4em" :style="level === 'medio' ? 'opacity: 1' : 'opacity: 0.4'" />
            <q-icon name="fiber_manual_record" color="green" size="4em" :style="level === 'bajo' ? 'opacity: 1' : 'opacity: 0.4'" />
            <p class="q-my-md" :class="texts[level]">Nivel de rayos uv: {{ level }}</p>
            <p class="q-my-md">
              <q-btn to="/info" color="accent" flat label="Más información" icon="info" />
            </p>
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
    connectionStatus: null,
    level: null,
    texts: {
      bajo: 'green',
      medio: 'amber',
      alto: 'red'
    }
  }),
  created () {
    // logout if not authenticated user
    // if (!localStorage.getItem('userId')) {
    //   this.$router.push('/login')
    // }
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

      setInterval(() => {
        this.saveQuery(this.base64ToStr(JSON.stringify(this.uvIndex)))
      }, 10000)
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

      this.level = level

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
