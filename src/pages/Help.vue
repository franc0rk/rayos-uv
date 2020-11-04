<template>
  <q-page class="row">
    <div class="col q-py-md">
      <h5 class="q-mx-lg q-my-sm">
        <q-icon name="help" color="accent" />
        Ayuda
      </h5>
      <q-list bordered padding separator class="q-mx-lg">
        <q-item clickable v-for="manual in ['user_manual', 'technical_manual']" :key="manual">
          <q-item-section avatar>
            <q-icon size="3em" :name="manual === 'user_manual' ? 'menu_book' : 'developer_mode'" color="accent" />
          </q-item-section>

          <q-item-section>
            <q-item-label>{{ manual === 'user_manual' ? 'Manual de usuario' : 'Manual técnico' }}</q-item-label>
            <q-item-label caption lines="1">
              Lorem ipsum dolor sit amet consectetur,
              adipisicing elit. Cupiditate, sit dicta,
              provident repellat voluptate officia modi,
              officiis ducimus veritatis,
              nesciunt dolore voluptatem eius mollitia
              quaerat error culpa aliquam eveniet ad.
            </q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'Help'
}
</script>
