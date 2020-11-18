<template>
  <q-page padding class="docs-table">
    <q-table
      ref="table"
      color="primary"
      title="Historial de consultas"
      :data="serverData"
      :columns="columns"
      row-key="id"
      @request="request"
      :loading="loading"
    >
    </q-table>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      serverData: [],
      columns: [
        {
          name: 'id',
          required: true,
          label: 'ID',
          align: 'left',
          field: row => row.id
        },
        { name: 'uv_index', label: 'Indice', field: 'uv_index', sortable: true },
        { name: 'role', label: 'Nivel', field: 'role', sortable: true },
        { name: 'date', label: 'Fecha de consulta', field: 'date', sortable: true }
      ],
      loading: false
    }
  },
  methods: {
    request (props) {
      this.loading = true
      setTimeout(() => {
        const id = localStorage.getItem('userId')
        if (id) {
          this.$axios.get(`https://uv-api.herokuapp.com/consultas/${id}`)
            .then((response) => {
              this.serverData = response.data
              this.loading = false
            })
            .catch(() => {
              this.serverData = []
            })
        } else {
          this.serverData = []
          this.$router.push('login')
        }
      }, 1500)
    }
  },
  mounted () {
    this.request({
      pagination: this.serverPagination,
      filter: this.filter
    })
  }
}
</script>
