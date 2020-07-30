<template>
  <div>
    <div class="row">
      <div class="card teacherName col-md-3">
        <p v-if="person!=null" class="mx-auto">Docente: {{person.TeacherFullName}}</p>
      </div>
    </div>

    <div class="row">
      <div class="col-md-12">
        <data-tables v-bind="{url, propsToSearch, tableColumns,pagination,actions: hasActions}">
          <template slot="buttons" slot-scope="props">
            <el-tooltip v-if="action==='MODIFY'" class="item" effect="dark" content="Modificar" placement="top-start">
              <a class="btn btn-simple btn-xs btn-icon btn-info" @click="$emit('Modify',props.queriedData[props.index].Id)"><i class="fa fa-edit"></i></a>
            </el-tooltip>
          </template>
        </data-tables>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    props: {
      personId: {
        type: Number,
        required: true
      }
    },
    data () {
      return {
        url: '/TeacherStudent/' + this.personId,
        propsToSearch: ['Modalidad', 'TipoTarea', 'Acta', 'ActaFecha', 'Carrera', 'Horas', 'MontoHora', 'TotalNeto', 'Deduccion', 'TotalBruto', 'Estudiante', 'MesLiteral', 'Gestion'],
        tableColumns: [
          {
            prop: 'Modalidad',
            label: 'Mod',
            minWidth: 15
          },
          {
            prop: 'TipoTarea',
            label: 'Tarea',
            minWidth: 15
          },
          {
            prop: 'Carrera',
            label: 'Carrera',
            minWidth: 20
          },
          {
            prop: 'Horas',
            label: 'Horas',
            minWidth: 15
          },
          {
            prop: 'MontoHora',
            label: 'Por Hora',
            minWidth: 15
          },
          {
            prop: 'TotalBruto',
            label: 'Total Bruto',
            minWidth: 20
          },
          {
            prop: 'Deduccion',
            label: 'Deduccion',
            minWidth: 20
          },
          {
            prop: 'TotalNeto',
            label: 'Total Neto',
            minWidth: 20
          },
          {
            prop: 'Estudiante',
            label: 'Estudiante',
            minWidth: 60
          },
          {
            prop: 'MesLiteral',
            label: 'Mes',
            minWidth: 10
          },
          {
            prop: 'Gestion',
            label: 'Gestion',
            minWidth: 15
          }
        ],
        pagination: {
          perPage: 5,
          currentPage: 1,
          perPageOptions: [5, 10, 20],
          total: 0
        },
        person: null,
        loaded: false
      }
    },
    methods: {
      loadTeacher () {
        axios.get('/AsesoriaDocente/' + this.personId, {
          headers: {
            'token': localStorage.getItem('token')
          }
        })
          .then(response => {
            this.person = response.data
            console.log(this.person)
          })
          .catch(error => console.log(error))
      }
    },
    computed: {
      hasActions: {
        get () {
          return this.action === 'MODIFY' || this.action === 'BAJA'
        }
      }
    },
    created () {
      this.loadTeacher()
    }
  }
</script>

<style scoped>
  .teacherName {
    font-weight: bold;
    font-size: 20px;
  }
</style>
