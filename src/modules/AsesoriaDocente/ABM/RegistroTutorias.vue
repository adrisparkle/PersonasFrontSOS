<template>
  <div v-if="!IsFetching" class="col-md-12">
    <div class="panel-body">
      <!--Carrera-->
      <div class="row">
        <div class="form-group col-md-6">
          <label>Carrera</label>
          <div>
            <model-select class="select-info"
                          v-bind:class="{fixI : action==='PUT'}"
                          :options="careers"
                          v-model="tutoria.Carrera"
                          placeholder="Seleccione una carrera">
            </model-select>
          </div>
        </div>
      </div>
      <!--Del Profesor-->
      <div class="row">
        <div class="form-group col-md-4">
          <label>Docente</label>
          <model-select class="select-info"
                        v-bind:class="{fixI : action==='PUT'}"
                        :options="teacherArray"
                        v-model="teacherIdentifier"
                        placeholder="Seleccionar docente"
                        :onchange="actualCat()"
          >
          </model-select>
        </div>
        <div class="form-group col-md-2">
          <label>Categoría</label>
          <model-select class="select-info"
                        v-bind:class="{fixI : action==='PUT'}"
                        :options="tipoDoc"
                        v-model="tutoria.Categoría"
                        placeholder="Categoría">
          </model-select>
        </div>
        <div class="form-group col-md-2">
          <label >Dependiente?</label>
          <input type="checkbox" class="form-control" v-model="dependiente" @click="filterTeachers()">
        </div>
        <div class="form-group col-md-2">
          <label >Otra Regional?</label>
          <input type="checkbox" class="form-control" v-model="or">
        </div>
        <div class="form-group col-md-2">
          <label >Por Hora?</label>
          <input type="checkbox" class="form-control" v-model="porHora">
        </div>
      </div>
      <!--Modalidad-->
      <div class="row">
        <div class="form-group col-md-3">
          <label>Modalidad</label>
          <div>
            <model-select class="select-info"
                          v-bind:class="{fixI : action==='PUT'}"
                          :options="modalidades"
                          v-model="tutoria.ModalidadId"
                          placeholder="Modalidad">
            </model-select>
          </div>
        </div>
        <div class="form-group col-md-3">
          <label>Tipo Tarea</label>
          <div>
            <model-select class="select-info"
                          v-bind:class="{fixI : action==='PUT'}"
                          :options="tipoTarea"
                          v-model="tutoria.TipoTareaId"
                          placeholder="Tipo de tarea">
            </model-select>
          </div>
        </div>
      </div>
      <!--Del Alumno y acta-->
      <div class="row">
        <div class="form-group col-md-6">
          <label >Nombre del Alumno</label>
          <input type="text" placeholder="Apellidos y nombres del estudiante" style="text-transform: uppercase;" class="form-control textBox" v-model="tutoria.StudentFullName">
          <small v-if="formError.Student.active" class="form-text text-muted text-danger">{{formError.Student.message}}</small>
        </div>
        <div class="form-group col-md-2">
          <label >Tiene acta?</label>
          <input type="checkbox" placeholder="Acta" class="form-control" v-model="acta">
        </div>
        <!--Si el acta existe, llenar datos-->
        <div v-if="acta">
          <div class="form-group col-md-2">
            <label >Acta</label>
            <input type="text" placeholder="N° Acta" class="form-control textBox" v-model="tutoria.Acta">
            <small v-if="formError.Acta.active" class="form-text text-muted text-danger">{{formError.Acta.message}}</small>
          </div>
          <div class="form-group col-md-2">
            <label> Fecha </label>
            <div class="row">
              <v-date-picker
                v-model="tutoria.ActaFecha"
                locale="es"
                class="calendarProp"
                :input-props='{
                style: "color:#A5A5A5;",
                placeholder: "dd/mm/yyyy"
                }'
              />
            </div>
          </div>
        </div>
      </div>
      <!--Observaciones-->
      <div class="row">
        <div class="form-group col-md-6">
          <label for="exampleFormControlTextarea1">Observaciones</label>
          <textarea class="form-control textBox" style="text-transform: uppercase;" id="exampleFormControlTextarea1" rows="2" v-model="tutoria.Observaciones"></textarea>
        </div>
        <div class="form-group col-md-2">
          <label >Ignorar repetido?</label>
          <input type="checkbox" class="form-control" v-model="tutoria.Ignore">
        </div>
      </div>
      <!--Costos-->
      <div class="row">
        <div class="form-group col-md-2">
          <label >Costo Hora</label>
          <input type="text" placeholder="C/H" class="form-control" v-model="tutoria.MontoHora" readonly>
        </div>
        <div class="form-group col-md-2">
          <label >Horas</label>
          <input type="text" placeholder="Horas" class="form-control" :onchange="hrsChange()" v-model="tutoria.Horas" :readonly="!porHora">
        </div>
        <div class="form-group col-md-2">
          <label >Total Bruto</label>
          <input type="text" placeholder="TB" class="form-control textBox" :readonly="porHora" v-model="totalBruto">
        </div>
        <div v-if="dependiente" class="form-group col-md-2">
          <label >Deduccion(%)</label>
          <input type="text" placeholder="%" class="form-control textBox" v-model="Deduccion">
        </div>
        <div v-if="!dependiente" class="form-group col-md-2">
          <label >IUE(%)</label>
          <input type="text" placeholder="%" class="form-control textBox" v-model="IUE">
        </div>
        <div v-if="!dependiente" class="form-group col-md-2">
          <label >IT(%)</label>
          <input type="text" placeholder="%" class="form-control textBox" v-model="IT">
        </div>
        <div class="form-group col-md-2">
          <label >Total Neto</label>
          <input type="text" placeholder="TN" class="form-control textBox" v-model="totalNeto" readonly>
        </div>
      </div>
      <!--Botón que envía la información del form-->
      <div class="row">
        <div class="col-md-2">
          <button class="btn btn-success btn-fill" @click="send()">Guardar</button>
        </div>
        <div class="col-md-2" v-if="action==='PUT'">
          <button class="btn btn-danger btn-fill" @click="onClose()">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  import Datepicker from 'vuejs-datepicker'
  import swal from 'sweetalert2'
  import { ModelSelect } from 'vue-search-select'
  import {FormWizard, TabContent} from 'vue-form-wizard'
  import 'vue-form-wizard/dist/vue-form-wizard.min.css'

  export default {
    components: {
      ModelSelect,
      Datepicker,
      FormWizard,
      TabContent
    },
    props: {
      tutoriaId: {
        type: Number,
        required: false
      },
      action: {
        // Por defecto es POST, y cuando haya que actualizar, la función send revisará el prop
        type: String,
        required: false,
        default: 'POST'
      },
      origin: {
        type: String,
        required: false,
        default: 'DEPEN'
      }
    },
    data: function () {
      return {
        IsFetching: true,
        totalBruto: 0,
        options: [],
        careers: [],
        branches: [],
        modalidades: [],
        tipoTarea: [],
        dependiente: true,
        or: false,
        Deduccion: 0,
        teacherIdentifier: '',
        teacherArray: [],
        IUE: 13,
        IT: 3,
        acta: true,
        porHora: true,
        monto: null,
        teachers: [],
        tipoDoc: [
          {value: 'LPZ-A', text: 'LPZ-A', pricing: 54.75},
          {value: 'LPZ-B', text: 'LPZ-B', pricing: 51.03},
          {value: 'LPZ-C', text: 'LPZ-C', pricing: 46.78},
          {value: 'CBB-A', text: 'CBB-A', pricing: 10},
          {value: 'TJA-U', text: 'TJA-U', pricing: 10},
          {value: 'EPC-U', text: 'EPC-U', pricing: 10}
        ],
        tutoria: {
          Id: null,
          Carrera: '',
          TeacherFullName: '',
          TeacherCUNI: '',
          TeacherBP: '',
          Categoría: '',
          BranchesId: null,
          DependencyCod: '',
          Acta: '',
          ActaFecha: null,
          Horas: null,
          MontoHora: null,
          TotalNeto: null,
          TotalBruto: null,
          StudentFullName: '',
          Mes: null,
          Gestion: null,
          Observaciones: '',
          Valid: false,
          Deduccion: null,
          Origen: 'DEPEN',
          ModalidadId: null,
          TipoTareaId: null,
          Ignore: false,
          IUE: null,
          IT: null
        },
        formError: {
          Acta: {
            active: false,
            message: '*Este valor no puede ser vacio.'
          },
          Student: {
            active: false,
            message: '*Este valor no puede ser vacio.'
          }
        }
      }
    },
    computed: {
      // --------------------------Para el cálculo de los montos----------------------------
      totalNeto: function () {
        // Los calculos cambian si es DEPENDIENTE O INDEPENDIENTE
        if (this.dependiente) {
          this.tutoria.TotalNeto = (this.totalBruto - (this.totalBruto * (this.Deduccion / 100))).toFixed(2)
          this.tutoria.Deduccion = this.tutoria.TotalBruto - this.tutoria.TotalNeto
          return (this.totalBruto - (this.totalBruto * (this.Deduccion / 100))).toFixed(2)
        } else {
          // Cuando es independiente se calculan nuevos tipos de descuentos que no aplican para los independientes
          this.tutoria.IUE = (this.totalBruto * (this.IUE / 100))
          this.tutoria.IT = (this.totalBruto * (this.IT / 100))
          this.tutoria.Deduccion = this.tutoria.IUE + this.tutoria.IT
          this.tutoria.TotalNeto = (this.totalBruto - this.tutoria.IUE - this.tutoria.IT).toFixed(2)
          return (this.totalBruto - this.tutoria.IUE - this.tutoria.IT).toFixed(2)
        }
      },
      brutoCalculado: function () {
        return this.tutoria.Horas * this.tutoria.MontoHora
      },
      lockDependiente: function () {
        if (this.or && !this.dependiente) {
          this.dependiente = false
        }
      }
    },
    methods: {
      // --------------------------Para la validación antes del POST/PUT----------------------------
      valid: function () {
        if (this.acta) {
          this.formError.Acta.active = this.isEmptyBlanckOrNull(this.tutoria.Acta)
        }
        this.formError.Student.active = this.isEmptyBlanckOrNull(this.tutoria.StudentFullName)
        var x = this.formError.Acta.active || this.formError.Student.active
        return x
      },
      isEmptyBlanckOrNull: function (val) {
        return !val || val.length === 0 || !val.toString().trim()
      },
      // --------------------------Para el cálculo de los montos-----------------------------------------
      hrsChange () {
        if (this.porHora) {
          this.tutoria.TotalBruto = this.brutoCalculado
          this.totalBruto = this.brutoCalculado
        } else {
          this.tutoria.TotalBruto = this.totalBruto
        }
      },
      // --------------------------Para encontrar y asignar datos del docente----------------------------
      filterTeachers: function () {
        let boolean
        if (this.dependiente) {
          boolean = '0'
        } else {
          boolean = '1'
        }
        this.teacherArray = this.teachers.filter(function (e) {
          return e.TipoPago === boolean
        })
      },
      actualCat: function () {
        for (var i = 0; i < this.teacherArray.length; i++) {
          // si es dependiente y el valor iguala, entonces se guarda en el CUNI, sino en el BP
          // resetea el otro identificador, para no guardar el identificador de un docente seleccionado antes
          if (this.dependiente && this.teacherArray[i]['value'] === this.teacherIdentifier) {
            this.tutoria.TeacherBP = ''
            this.tutoria.TeacherCUNI = this.teacherArray[i]['value']
            this.tutoria.TeacherFullName = this.teacherArray[i]['name']
            this.tutoria.Categoría = this.teacherArray[i]['Categoria']
            // Aquí asignamos el precio según la categoría
            this.categoryChange(this.tutoria.Categoría)
          }
          if (!this.dependiente && this.teacherArray[i]['value'] === this.teacherIdentifier) {
            console.log(this.teacherArray[i]['value'])
            this.tutoria.TeacherCUNI = ''
            this.tutoria.TeacherBP = this.teacherArray[i]['value']
            this.tutoria.TeacherFullName = this.teacherArray[i]['name']
            this.tutoria.Categoría = this.teacherArray[i]['Categoria']
            console.log('INDEPENDEITNEs')
            console.log(this.tutoria.TeacherBP)
            console.log(this.tutoria.TeacherFullName)
            this.categoryChange(this.tutoria.Categoría)
          }
        }
        return this.tutoria.Categoría
      },
      categoryChange (cat) {
        // Carga el monto hora, que es el precio del array "tipo doc" que haga match con la categoría seleccionada
        for (var i = 0; i < this.tipoDoc.length; i++) {
          if (this.tipoDoc[i]['value'] === cat) {
            var x = this.tipoDoc[i]['pricing']
          }
        }
        this.tutoria.MontoHora = x
      },
      // ----------------------------------------Cargar datos desde BD----------------------------------------
      fakeLoad () {
        this.$store.commit('crud/loadSetter', true)
        setTimeout(() => {
          this.$store.commit('crud/loadSetter', false)
        }, 500)
      },
      // Usamos esta función en el PUT, para cargar los datos actuales del registro
      loadTutoria: function () {
        axios.get('AsesoriaDocente/' + this.tutoriaId)
          .then(response => {
            this.tutoria = response.data
            // Dependiendo dl origen del docente se carga el CUNI o el BP
            if (this.tutoria.Origen === 'DEPEN' || this.tutoria.Origen === 'OR') {
              // para mostrar al docente del registro
              console.log('CUNI is loaded')
              this.teacherIdentifier = this.tutoria.TeacherCUNI
              console.log('aqui businees partner. ' + this.teacherIdentifier + '=' + this.tutoria.TeacherCUNI)
              console.log('DEPEN' + this.teacherIdentifier)
              if (this.tutoria.Origen === 'OR') {
                console.log('OR is set to true')
                this.or = true
              }
              // para igualar costos, es necesario hacer un cálculo inverso porque nosotros guardamos monto pero desplegamos porcentaje
              this.Deduccion = ((100 * this.tutoria.Deduccion) / this.tutoria.TotalBruto).toFixed(2)
            } else if (this.tutoria.Origen === 'INDEP') {
              this.teacherIdentifier = this.tutoria.TeacherBP
              console.log('aqui businees partner. ' + this.teacherIdentifier + '=' + this.tutoria.TeacherBP)
              console.log('INDEP' + this.tutoria.TeacherFullName)
              console.log('INDEP' + this.teacherIdentifier)
              this.dependiente = false
              // para igualar costos
              this.IUE = ((100 * this.tutoria.IUE) / this.tutoria.TotalBruto).toFixed(2)
              this.IT = ((100 * this.tutoria.IT) / this.tutoria.TotalBruto).toFixed(2)
            }
            if (this.tutoria.Horas <= 0) {
              // para que no oculte el valor de la edición
              this.porHora = false
              // para que no cargue el total bruto ni el neto desde 0
              this.totalBruto = this.tutoria.TotalBruto
            }
            if (this.tutoria.Acta === '') {
              this.acta = false
            }
            // La fecha solo la tienen algunos registros, el cargado de la fecha será opcional
            if (this.tutoria.ActaFecha != null) {
              var actaYear = this.tutoria.ActaFecha.substring(0, 4)
              var actaMonth = this.tutoria.ActaFecha.substring(5, 7)
              var actaDay = this.tutoria.ActaFecha.substring(8, 10)
              var date = new Date(actaYear, actaMonth - 1, actaDay)
              this.tutoria.ActaFecha = date
            }
            this.IsFetching = false
          })
          .catch(error => { console.log('el error es:' + error) })
      },
      loadCareers () {
        let carreras = this.careers
        axios.get('CostCenters/Careers')
          .then(response => {
            response.data.forEach(function (element) {
              carreras.push({value: element.PrcCode, text: element.PrcCode + '-' + element.PrcName, BranchesId: element.BranchesId, Id: element.Id})
            })
          })
          .catch(error => console.log(error))
        this.IsFetching = false
      },
      loadTeachers () {
        let teachers = this.teachers
        // Este array guarda la info de los profesores que se cargan la primera vez
        let firstTeachers = this.teacherArray
        let selectedOrigin = '1'
        if (this.origin === 'INDEP') {
          console.log('This is the PUT action and the selected origin is:' + selectedOrigin)
          selectedOrigin = '0'
        }
        axios.get('DocentesList')
          .then(response => {
            response.data.forEach(function (element) {
              // Se envío el BP como CUNI para que solo llegara un valor en el value, que se asignará después a teacherCUNI o teacherBP dependiendo de su origen
              teachers.push({value: element.CUNI, text: element.CUNI + '-' + element.FullName, name: element.FullName, TipoPago: element.TipoPago, Categoria: element.Categoria})
             // console.log('Terachers' + element.CUNI + '-' + element.FullName)
              // Creamos un array con los docentes dependientes
              if (element.TipoPago === selectedOrigin) {
                console.log('This is the update action: ' + selectedOrigin)
                firstTeachers.push({value: element.CUNI, text: element.CUNI + '-' + element.FullName, name: element.FullName, TipoPago: element.TipoPago, Categoria: element.Categoria})
                // console.log('FirstTeachers' + element.CUNI + '-' + element.FullName)
              }
            })
            console.log('el array de los profesores es:')
            console.log(firstTeachers)
          })
          .catch(error => console.log(error))
        this.IsFetching = false
      },
      loadModalidades () {
        let mods = this.modalidades
        axios.get('Modalidades')
          .then(response => {
            response.data.forEach(function (element) {
              mods.push({value: element.Id, text: element.Modalidad})
            })
          })
          .catch(error => console.log(error + 'Im here cause I messed up'))
      },
      loadTipoTarea () {
        let tipot = this.tipoTarea
        axios.get('TipoTarea')
          .then(response => {
            response.data.forEach(function (element) {
              tipot.push({value: element.Id, text: element.Tarea})
            })
          })
          .catch(error => console.log(error + 'Im here cause I messed up'))
      },
      // Envío de datos
      send () {
        if (!this.valid() && this.action === 'POST') {
          console.log('Algo si entrá al POST')
          this.post()
        } else if (!this.valid() && this.action === 'PUT') {
          console.log('Algo si entrá al PUT')
          this.put()
        } else {
          console.log('something was printed:' + this.action + ' ' + this.valid())
          // resetear variables de validación
          this.formError.Acta.active = false
          this.formError.Student.active = false
          this.validationMessage()
        }
      },
      cleanScreen () {
        // este método limpia la pantalla, con excepcion del porcentaje y la carrera
        // Variables del objeto que se envía mediante el POST
        this.tutoria.DependencyCod = ''
        this.tutoria.Observaciones = ''
        this.tutoria.TotalNeto = null
        this.tutoria.MontoHora = null
        this.tutoria.Horas = null
        this.tutoria.Categoría = ''
        this.tutoria.TotalBruto = null
        this.totalBruto = 0
        this.tutoria.TotalNeto = null
        this.tutoria.Acta = ''
        this.tutoria.ActaFecha = null
        this.tutoria.TeacherFullName = ''
        this.tutoria.TeacherBP = ''
        this.tutoria.TeacherCUNI = ''
        this.tutoria.ModalidadId = null
        this.tutoria.TipoTareaId = null
        this.tutoria.StudentFullName = ''
        // Variables del componente
        this.Deduccion = 0
        this.IUE = 0
        this.IT = 0
        // Variables de control de errores
        this.formError.Acta.active = false
        this.formError.Student.active = false
      },
      post () {
        axios.post('AsesoriaDocente', this.tutoria)
          .then(response => {
            console.log('Tutoria aqui' + this.tutoria.TeacherFullName)
            this.successMessage()
            this.cleanScreen()
          })
          .catch(error => {
            this.errorMessage(error.response.data.Message)
            console.log(error.response.data.Message)
          })
      },
      // Actualización de datos
      put () {
        console.log('AsesoriaDocente/' + this.tutoriaId, this.tutoria)
        axios.put('AsesoriaDocente/' + this.tutoriaId, this.tutoria)
          .then(response => {
            this.successMessage()
            this.cleanScreen()
            this.onClose()
          })
          .catch(error => {
            this.errorMessage(error.response.data.Message)
          })
      },
      onClose () {
        // emit para cerrar el componente
        this.$emit('back')
      },
      successMessage: function () {
        swal({
          title: `Buen trabajo!`,
          text: 'Se guardaron los cambios!',
          buttonsStyling: false,
          confirmButtonClass: 'btn btn-success btn-fill',
          type: 'success'
        })
      },
      errorMessage: function (text) {
        swal({
          title: `Ups!`,
          text: 'Ocurrio un error!\n' + text,
          buttonsStyling: false,
          confirmButtonClass: 'btn btn-success btn-fill',
          type: 'error'
        })
      },
      validationMessage: function () {
        swal({
          title: `Ups!`,
          text: 'Algunos datos no están validados',
          buttonsStyling: false,
          confirmButtonClass: 'btn btn-success btn-fill',
          type: 'error'
        })
      }
    },
    watch: {
      porHora: function () {
        // Para que el dato de la tutoria no se quede con el valor anterior si fue marcada como "por horas"
        if (!this.porHora) { this.tutoria.Horas = 0 }
      },
      acta: function () {
        if (!this.acta) {
          this.tutoria.Acta = ''
          this.tutoria.ActaFecha = null
        }
      },
      or: function () {
        if (this.or) {
          // Si no es dependiente no puede ser OR
          if (!this.dependiente) {
            this.or = false
          } else {
            // Pero si lo es, entonces el origen es OR
            this.tutoria.Origen = 'OR'
          }
        } else {
          if (this.dependiente) {
            this.tutoria.Origen = 'DEPEN'
          }
        }
      },
      dependiente: function () {
        if (this.dependiente) {
          if (!this.or) {
            this.tutoria.Origen = 'DEPEN'
          }
        }
        if (!this.dependiente) {
          this.tutoria.Origen = 'INDEP'
          // si no es dependiente no puede ser OR
          this.or = false
        }
        // borrar la categoría actual
        this.tutoria.Categoría = ''
        // borramos los datos del docente
        this.tutoria.TeacherFullName = ''
        this.tutoria.TeacherCUNI = ''
        this.tutoria.TeacherBP = ''
        this.tutoria.IUE = null
        this.tutoria.IT = null
      }
    },
    created () {
      if (this.action === 'PUT') {
        // Solo cargamos los datos de la tutoría cuando hay un PUT, sino cargamos todo menos eso
        this.fakeLoad()
        this.loadTutoria()
        this.loadModalidades()
        this.loadTipoTarea()
        this.loadTeachers()
        this.loadCareers()
      } else {
        this.loadModalidades()
        this.loadTipoTarea()
        this.loadTeachers()
        this.loadCareers()
      }
    }
  }
</script>

<style scoped>
  .fixI i:link, .fixI i {
    top: 25px!important;
  }
  .calendarProp {
    color: red;
  }
  .textBox {
    background-color: white;
    text-transform: uppercase;
  }
</style>
