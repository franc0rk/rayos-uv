<template>
  <q-page class="row">
    <div class="col q-py-md">
      <h5 class="q-mx-lg q-my-sm">
        <q-icon name="info" color="accent" />
        Información
      </h5>
      <q-list bordered padding separator class="q-mx-lg">
        <q-item clickable v-for="color in ['green', 'amber', 'red']" :key="color">
          <q-item-section avatar>
            <q-icon size="3em" name="fiber_manual_record" :color="color" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Semáforo {{ color }}</q-item-label>
            <q-item-label caption lines="8">
              Lorem ipsum dolor sit amet consectetur,
              adipisicing elit. Cupiditate, sit dicta,
              provident repellat voluptate officia modi,
              officiis ducimus veritatis,
              nesciunt dolore voluptatem eius mollitia
              quaerat error culpa aliquam eveniet ad.
            </q-item-label>
          </q-item-section>

          <q-item-section side>
            <q-btn round flat icon="add" color="accent" />
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageInfo'
}
</script>
