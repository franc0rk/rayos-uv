<template>
  <q-page class="row">
    <div class="col q-py-md">
      <h5 class="q-mx-lg q-my-sm">
        <q-icon name="info" color="accent" />
        Información
      </h5>
      <q-list bordered padding separator class="q-mx-lg">
        <q-item clickable v-for="color in colors" :key="color.name" class="q-py-lg">
          <q-item-section avatar>
            <q-icon size="3em" name="fiber_manual_record" :color="color.name" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Semáforo {{ color.name_es }}</q-item-label>
            <q-item-label caption lines="8">
              {{ color.title }}
            </q-item-label>
          </q-item-section>

          <q-item-section side>
            <q-btn round flat icon="add" color="accent" @click="openColor(color)" />
          </q-item-section>
        </q-item>
      </q-list>
    </div>
    <q-dialog v-model="opened" v-if="openedColor" @hide="opened = false">
      <q-card style="width: 400px;">
        <q-card-section>
          <div class="text-subtitle1" :class="'text-' + openedColor.name">
            Recomendaciones - Semáforo {{ openedColor.name_es }}
          </div>
          <ul>
            <li class="q-px-sm q-py-md" v-for="(point, pointIndex) in openedColor.points" :key="pointIndex">
              {{ point }}
            </li>
          </ul>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
export default {
  name: 'PageInfo',
  data: () => ({
    opened: false,
    openedColor: null,
    colors: [
      {
        name: 'green',
        name_es: 'Verde',
        title: 'Una lectura de índice UV del 0 al 4 significa bajo peligro de los rayos UV del sol para una persona promedio.',
        points: [
          'Utilice anteojos de sol los días de sol brillante.',
          'Si se quema con facilidad, cúbrase y use un protector solar de amplio espectro SPF 30+.',
          'Tenga cuidado con las superficies brillantes, como arena, agua y nieve, que reflejan los rayos UV y aumentan la exposición.'
        ]
      },
      {
        name: 'amber',
        name_es: 'Amarillo',
        title: 'Una lectura de índice UV de 5 a 7 significa un riesgo moderado de daño por exposición al sol sin protección.',
        points: [
          'Permanezca a la sombra cerca del mediodía, cuando el sol está más fuerte.',
          'Si está al aire libre, utilice ropa de protección, un sombrero de ala ancha y anteojos de sol que bloqueen los rayos UV.',
          'Aplíquese generosamente un protector solar de amplio espectro SPF 30+ cada 2 horas, incluso si está nublado, y después de nadar o sudar. Tenga cuidado con las superficies brillantes, como arena, agua y nieve, que reflejan los rayos UV y aumentan la exposición.'
        ]
      },
      {
        name: 'red',
        name_es: 'Rojo',
        title: 'Una lectura de índice UV de 8 a 11 significa un riesgo alto de daño por exposición al sol sin protección. Es necesario protegerse la piel y los ojos para que no sufran daños.',
        points: [
          'Reduzca el tiempo al sol entre las 10 a. m y las 4 p. m.',
          'Si está al aire libre, busque la sombra y utilice ropa de protección, un sombrero de ala ancha y anteojos de sol que bloqueen los rayos UV.',
          'Aplíquese generosamente un protector solar de amplio espectro SPF 30+ cada 2 horas, incluso si está nublado, y después de nadar o sudar. Tenga cuidado con las superficies brillantes, como arena, agua y nieve, que reflejan los rayos UV y aumentan la exposición.'
        ]
      }
    ]
  }),
  methods: {
    openColor (color) {
      this.openedColor = color
      this.opened = true
    }
  }
}
</script>
