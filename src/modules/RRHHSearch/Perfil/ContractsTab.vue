<template>
  <div style="background-color: ghostwhite">
    <br>
    <button v-if="action==='BAJA'" class="btn btn-danger pull-right" @click="$emit('Remove','ALL')">Baja</button>
    <br>
    <data-tables v-bind="{url, propsToSearch, tableColumns,pagination,actions: hasActions}">
    </data-tables>
  </div>
</template>

<script>
  export default {
    props: {
      personId: {
        type: Number,
        required: true
      },
      action: {
        type: String,
        required: true
      }
    },
    computed: {
      hasActions: {
        get () {
          return this.action === 'MODIFY' || this.action === 'BAJA'
        }
      }
    },
    data () {
      return {
        url: '/people/Contracts/' + this.personId,
        propsToSearch: ['CUNI', 'Document', 'FirstSurName', 'SecondSurName', 'MariedSurName', 'Names'],
        tableColumns: [
          {
            prop: 'Branches',
            label: 'Regional',
            minWidth: 100
          },
          {
            prop: 'Cod',
            label: 'Cod.',
            minWidth: 80
          },
          {
            prop: 'Dependency',
            label: 'Dependencia',
            minWidth: 200
          },
          {
            prop: 'Positions',
            label: 'Cargo',
            minWidth: 150
          },
          {
            prop: 'Dedication',
            label: 'Dedicación',
            minWidth: 150
          },
          {
            prop: 'Link',
            label: 'Vinculacion',
            minWidth: 150
          },
          {
            prop: 'StartDatestr',
            label: 'Desde',
            minWidth: 120
          },
          {
            prop: 'EndDatestr',
            label: 'Hasta',
            minWidth: 120
          }
        ],
        pagination: {
          perPage: 50,
          currentPage: 1,
          perPageOptions: [5, 10, 20],
          total: 0
        }
      }
    }
  }
</script>

<style scoped>

</style>
