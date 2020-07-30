<template>
  <div class="container col-md-12">
    <template>
      <div class="row">
        <div class="form-group col-md-4">
          <label>Tipo Documento</label>
          <div>
            <input type="text" placeholder="Tipo Documento" class="form-control" readonly v-model="person.TypeDocument">
          </div>
        </div>

        <div class="form-group col-md-4">
          <label>Numero Documento</label>
          <div>
            <input type="text" placeholder="Documento de identidad" class="form-control" readonly v-model="person.Document">
          </div>
        </div>

        <template v-if="person.TypeDocument==='CI'">
          <div class="form-group col-md-4">
            <label>Expedido en </label>
            <div>
              <input type="text" placeholder="Expedido" class="form-control" readonly v-model="person.Ext">
            </div>
          </div>
        </template>
      </div>
      <div class="row">
        <div class="el-col-24"><span style="font-size: 30px;">{{fullName}}</span></div>

        <div class="form-group col-md-6">
          <label>Nombres</label>
          <div>
            <input type="text" placeholder="Nombres" class="form-control" readonly v-model="person.Names">
          </div>
        </div>

        <div class="form-group col-md-6">
          <label>Primer Apellido</label>
          <div>
            <input type="text" placeholder="Primer Apellido" class="form-control" readonly v-model="person.FirstSurName">
          </div>
        </div>

      </div>
      <div class="row">
        <div class="form-group col-md-6">
          <label>Segundo Apellido</label>
          <div>
            <input type="text" placeholder="Segundo Apellido" class="form-control" readonly v-model="person.SecondSurName">
          </div>
        </div>
        <template v-if="person.Gender==='F'">
          <template v-if="person.UseMariedSurName">
            <div class="form-group col-md-6">
              <label>Apellido Casada</label>
              <div>
                <input type="text" placeholder="Apellido Casada" class="form-control" readonly v-model="person.MariedSurName">
              </div>
            </div>
          </template>

        </template>


      </div>
      <div class="row">
        <div class="form-group col-md-3">
          <label>Genero</label>
          <div>
            <input type="text" placeholder="Genero" class="form-control" readonly v-model="person.Gender">
          </div>
        </div>
        <div class="form-group col-md-3">
          <label>Fecha de Nacimiento</label>
          <div>
            <input type="text" placeholder="Fecha de Nacimiento" class="form-control" readonly v-model="BrithDateStr">
          </div>
        </div>
        <div class="form-group col-md-3">
          <label>Edad</label>
          <div>
            <input type="text" placeholder="Edad" class="form-control" readonly v-model="person.Age">
          </div>
        </div>
        <div class="form-group col-md-3">
          <label>Nacionalidad</label>
          <div>
            <input type="text" placeholder="Nacionalidad" class="form-control" readonly v-model="person.Nationality">
          </div>
        </div>
      </div>
      <div class="row">
        <div class="form-group col-md-4">
          <label>AFP</label>
          <div>
            <input type="text" placeholder="AFP" class="form-control" readonly v-model="person.AFP">
          </div>
        </div>
        <div class="form-group col-md-4">
          <label>NUA</label>
          <div>
            <input type="text" placeholder="NUA" class="form-control" readonly v-model="person.NUA">
          </div>
        </div>
        <div class="form-group col-md-4">
          <label>Seguro</label>
          <div>
            <input type="text" placeholder="Seguro" class="form-control" readonly v-model="person.Insurance">
          </div>
        </div>
      </div>
      <div class="row">
        <div class="form-group col-md-6">
          <label>Email institucional</label>
          <div>
            <input type="text" placeholder="UCB Email" class="form-control" readonly v-model="person.UcbEmail">
          </div>
        </div>

        <div class="form-group col-md-6">
          <label>Email Personal</label>
          <div>
            <input type="text" placeholder="Email Personal" class="form-control" readonly v-model="person.PersonalEmail">
          </div>
        </div>
      </div>

      <!--
      <el-tooltip class="item" effect="dark" content="Descargar CI" placement="top-start">
        <a class="btn circular btn-simple btn-icon btn-info pull-right btn-fill" @click="getCI()">
          <i class="fa fa-id-card" style=""></i>
        </a>
      </el-tooltip>-->
    </template>
  </div>

</template>

<script>
  import Datepicker from 'vuejs-datepicker'
  import {en, es} from 'vuejs-datepicker/dist/locale'
  import axios from 'axios'
  import swal from 'sweetalert2'

  export default {
    props: {
      person: {
        type: Object,
        required: true
      }
    },
    computed: {
      canEdit: function () {
        let access = JSON.parse(localStorage.getItem('Menu'))
        let found = access.find(function (element) {
          return element.name === 'GPS'
        })
        let child = found.children.find(function (element) {
          return element.name === 'Historial Empleados'
        })
        return child.methods.includes('PUT')
      },
      fullName: function () {
        let fn = this.Names != null ? this.Names : ''
        if (this.FirstSurName != null) {
          fn += ' ' + this.FirstSurName
        }
        if (this.UseSecondSurName && this.SecondSurName != null) {
          fn += ' ' + this.SecondSurName
        }
        if (this.UseMariedSurName && this.MariedSurName != null) {
          fn += ' ' + this.MariedSurName
        }
        return fn.toUpperCase()
      },
      BrithDateStr: function () {
        let strArray = ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic']
        let d = new Date(this.person.BirthDate)
        if (isNaN(d.getTime())) {
          return 'Sin Fecha'
        }
        let str = ('0' + d.getDate()).slice(-2) + '-' + (strArray[d.getMonth()]) + '-' + d.getFullYear()
        return str
      },
      fileName: {
        get () {
          return this.file === null ? 'No se selecciono ningun archivo' : this.file.name
        }
      }
    },
    data () {
      return {
        file: null,
        refresh: false,
        update: false,
        en: en,
        es: es,
        initialview: 'year',
        format: 'dd-MMM-yyyy',
        opendate: new Date(1975, 5, 1),
        url: '/people',
        selectLevel: {
          select: '',
          values: []
        },
        formData: {
          UseSecondSurName: true,
          UseMariedSurName: false,
          CUNI: null,
          TypeDocument: null,
          Document: null,
          Ext: null,
          FirstSurName: null,
          SecondSurName: null,
          Names: null,
          MariedSurName: null,
          BirthDate: null,
          Gender: null,
          Nationality: null,
          NUA: null,
          AFP: null
        },
        formError: {
          UseSecondSurName: false,
          UseMariedSurName: false,
          CUNI: false,
          TypeDocument: false,
          Document: false,
          Ext: false,
          FirstSurName: false,
          SecondSurName: false,
          Names: false,
          MariedSurName: false,
          BirthDate: false,
          Gender: false,
          Nationality: false,
          NUA: false,
          AFP: false,
          Pending: false,
          file: false
        },
        selectGender: {
          select: '',
          values: [{Id: 'M', Name: 'Masculino'},
            {Id: 'F', Name: 'Femenino'}]
        },
        selectDocumentType: {
          select: '',
          values: [{Id: 'CI', Name: 'Carnet de Identidad'},
            {Id: 'CE', Name: 'Carnet Extranjero'},
            {Id: 'PA', Name: 'Pasaporte'}]
        },
        selectNationality: {
          select: '',
          values: [{Id: 'NACIONAL', Name: 'Nacional'},
            {Id: 'EXTRANJERO', Name: 'Extranjero'}]
        },
        selectAFP: {
          select: '',
          values: [{Id: 'PREV', Name: 'Prevision'},
            {Id: 'FUT', Name: 'Futuro'}]
        },
        selectIssued: {
          select: '',
          values: [{Id: 'LP', Name: 'La Paz'},
            {Id: 'CB', Name: 'Cochabamba'},
            {Id: 'SC', Name: 'Santa Cruz'},
            {Id: 'CH', Name: 'Chuquisaca'},
            {Id: 'TJ', Name: 'Tarija'},
            {Id: 'OR', Name: 'Oruro'},
            {Id: 'PT', Name: 'Potosi'},
            {Id: 'BN', Name: 'Beni'},
            {Id: 'PA', Name: 'Pando'}
          ]
        }
      }
    },
    components: {
      Datepicker
    },
    methods: {
      getCI: function () {
        axios.get('/people/UploadCI/' + this.person.Id,
          {
            responseType: 'arraybuffer',
            headers: {
              'token': localStorage.getItem('token')
            }
          }
        )
          .then(response => {
            var filename = response.request.getResponseHeader('Content-Disposition')
            let dynamicType = filename.endsWith('.pdf') ? 'application/pdf' : 'image/bmp'
            const blob = new Blob([response.data], {
              type: dynamicType
            })
            const url = window.URL.createObjectURL(blob)
            const link = document.createElement('a')
            link.href = url
            // link.setAttribute('download', filename.split('filename=')[1])
            window.open(url, filename)
            // document.body.appendChild(link)
            // link.click()
          })
          .catch(error => {
            console.log(error)
            this.errorMessage('Esta persona no tiene respaldo de su Documento')
          })
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
      valid: function () {
        let x = true
        this.formError.Pending = !this.person.canUpdatePending
        x = this.formError.Pending
        return x
      },
      submitFile: function () {
        let entorno = this
        let formData1 = new FormData()
        formData1.append('file', this.file)
        axios.post('/people/UploadCI/' + this.person.Id,
          formData1,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
        ).then(function () {
          entorno.$store.commit('crud/loadSetter', false)
          entorno.successMessage()
        }).catch(error => {
          // handle error
          entorno.$store.commit('crud/loadSetter', false)
          entorno.errorMessage(error.response.data.Message)
        })
      },
      send: function () {
        let entorno = this
        this.$store.commit('crud/loadSetter', true)
        this.person.UseMariedSurName = this.person.UseMariedSurName ? 1 : 0
        this.person.UseSecondSurName = this.person.UseSecondSurName ? 1 : 0
        axios.put(this.url + '/' + this.person.Id, this.person, {
          headers: {
            'token': localStorage.getItem('token')
          }
        })
          .then(response => {
            console.log(response.data)
            entorno.CUNI = response.data.CUNI
            entorno.Document = response.data.Document
            entorno.Names = response.data.Names
            entorno.FirstSurName = response.data.FirstSurName
            entorno.SecondSurName = response.data.SecondSurName
            entorno.MariedSurName = response.data.MariedSurName
            entorno.UseMariedSurName = response.data.UseSecondSurName
            entorno.UseSecondSurName = response.data.UseSecondSurName
            entorno.$store.commit('crud/loadSetter', false)
            if (this.file === null) {
              entorno.successMessage()
            } else {
              this.submitFile()
            }
          })
          .catch(error => {
            console.log(error)
            entorno.$store.commit('crud/loadSetter', false)
            entorno.errorMessage(error.response.data)
          })
      },
      handleFileUpload () {
        let ext = this.$refs.file.files[0].name.split('.').pop()
        if (ext !== 'png' && ext !== 'jpg' && ext !== 'pdf') {
          this.formError.file = true
        } else {
          this.formError.file = false
          this.file = this.$refs.file.files[0]
        }
      },
      resetFile () {
        this.file = null
        const input = this.$refs.file
        input.type = 'text'
        input.type = 'file'
      }
    }
  }
</script>

<style scoped>
  .btn-file-custom input[type=file] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    font-size: 100px;
    text-align: right;
    filter: alpha(opacity=0);
    opacity: 0;
    outline: none;
    background: white;
    cursor: inherit;
    display: block;

  }
</style>
