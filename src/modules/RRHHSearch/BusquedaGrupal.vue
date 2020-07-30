<template>
<div class="col-lg-12 col-md-12 col-sm-12">
  <div class="col-lg-12 col-md-12 col-sm-12" style="background-color: ghostwhite">
    <template v-if="actiones ==='inicio'">
      <div class="el-col-offset-4 col-lg-8 col-md-12 col-sm-12" style="background-color: floralwhite">
        <h4>Buscar</h4>
        <form>
          <div class="row">
            <div class="col-lg-6 col-md-6 col-sm-6">
              <!--Regional-->
              <div class="form-group">
                <label>Regional *</label>
                <div align="center">
                  <el-select multiple
                             name="regional1"
                             filterable
                             size="large"
                             class="sele"
                             style="height: 40%; width: 95%;"
                             v-model="RegionalModel"
                             placeholder="Seleccione regional">
                    <el-option v-for="option in RegionalResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Unidad Organizacional-->
              <div class="form-group">
                <label>Unidad Organizacional</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="UOModel"
                             placeholder="Seleccione unidad organizacional">
                    <el-option v-for="option in UOResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.key">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Posicion-->
              <div class="form-group">
                <label>Posición</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="PosicionModel"
                             placeholder="Seleccione posición">
                    <el-option v-for="option in PosicionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Dedicacion-->
              <div class="form-group">
                <label>Dedicación</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="DedicacionModel"
                             placeholder="Seleccione posición">
                    <el-option v-for="option in DedicacionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Fecha inicio-->
              <div class="form-group">
                <label>Fecha inicio</label>
                <div align="center">
                  <datepicker :typeable="false"
                              :bootstrap-styling="true"
                              :initialView="initialview"
                              :format="format"
                              :language="es"
                              typeable
                              placeholder="Inicio"
                              :open-date="opendate"
                              v-model="fechaInicio">
                  </datepicker>
                  <!--
                  <small v-if="formError.BirthDate" class="form-text text-muted text-danger">*Selecciona un valor.</small>
                  -->
                </div>
              </div>
            </div>
            <div class="col-lg-6 col-md-6 col-sm-6">
              <!--Padre-->
              <div class="form-group">
                <label>Padre</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="ParentModel"
                             placeholder="Seleccione padre">
                    <el-option v-for="option in ParentResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.key">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Dependencia-->
              <div class="form-group">
                <label>Dependencia</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="DependenciaModel"
                             placeholder="Seleccione dependencia">
                    <el-option v-for="option in DependenciaResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Vinculacion-->
              <div class="form-group">
                <label>Vinculación</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="VinculacionModel"
                             placeholder="Seleccione vinculación">
                    <el-option v-for="option in VinculacionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Radio button-->
              <div class="form-group">
                <table>
                  <tr>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Activos">Activos</label><input type="radio" class="form-control" id="Activos" value="Activo" v-model="picked"></td>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Inactivos">Inactivos</label><input type="radio" class="form-control" id="Inactivos" value="Inactivo" v-model="picked"></td>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Todos">Todos</label><input type="radio" class="form-control" id="Todos" value="Todos" v-model="picked"></td>
                  </tr>
                </table>



              </div>
              <!--Fecha fin-->
              <div class="form-group">
                <label>Fecha fin</label>
                <div align="center">
                  <datepicker :typeable="false"
                              :bootstrap-styling="true"
                              :initialView="initialview"
                              :format="format"
                              :language="es"
                              typeable
                              placeholder="Fin"
                              :open-date="opendate"
                              v-model="fechaFin">
                  </datepicker>
                  <!--
                  <small v-if="formError.BirthDate" class="form-text text-muted text-danger">*Selecciona un valor.</small>
                  -->
                </div>
              </div>
            </div>
          </div>
          <br>
          <!--Boton-->
          <div class="el-col-pull-6 col-lg-12 col-md-12 col-sm-12" align="center">
            <button type="button" style="margin-right: 25px; margin-left: 25px" class="btn btn-lg btn-fill btn-success pull-right"
                       @click="check(RegionalModel,UOModel, ParentModel,DependenciaModel,PosicionModel,DedicacionModel,VinculacionModel,picked,fechaInicio,fechaFin)">
            <span class="btn-label ">
              <i class="fa el-icon-search fa-lg" style="color: white"></i>
            </span>
              Buscar
          </button>
            <button type="button" style="margin-right: 25px; margin-left: 25px" class="btn btn-lg btn-fill btn-warning pull-right"
                    @click="resetForm">
              <span class="btn-label ">
              <i class="fa el-icon-delete fa-lg" style="color: white"></i>
            </span>
              Reestablecer
            </button>
          <br><br></div>
        </form>
      </div>
    </template>
    <template v-if="actiones ==='busqueda'">
    <div class="row">
      <div class="col-lg-9 col-md-12 col-sm-12" style="background-color: ghostwhite">
        <data-tables :url="url" :propsToSearch="propsToSearch" :tableColumns="tableColumns" :pagination="pagination">
          <template slot="buttons" slot-scope="props">
            <el-tooltip class="item" effect="dark" :content="ButtonMessage1" placement="top-start">
              <a class="btn btn-simple btn-xs btn-icon btn-info" @click="DetailContract(props.queriedData[props.index].Id)"><i class="ti-bag" style=""></i></a>
            </el-tooltip><br>
            <el-tooltip class="item" effect="dark" :content="ButtonMessage2" placement="top-start">
              <a class="btn btn-simple btn-xs btn-icon btn-info" @click="DetailPerson(props.queriedData[props.index].Id)"><i class="fa fa-user-tie" style=""></i></a>
            </el-tooltip>
          </template>
        </data-tables>
      </div>
      <div class="col-lg-3 col-md-12 col-sm-12" style="background-color: ghostwhite">
        <h4>Buscar</h4>
        <form>
          <div class="row">
              <!--Regional-->
              <div class="form-group">
                <label>Regional *</label>
                <div align="center">
                  <el-select multiple
                             name="regional1"
                             filterable
                             size="large"
                             class="sele"
                             style="height: 40%; width: 95%;"
                             v-model="RegionalModel"
                             placeholder="Seleccione regional">
                    <el-option v-for="option in RegionalResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Unidad Organizacional-->
              <div class="form-group">
                <label>Unidad Organizacional</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="UOModel"
                             placeholder="Seleccione unidad organizacional">
                    <el-option v-for="option in UOResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.key">
                    </el-option>
                  </el-select>
                </div>
              </div>
            <!--Padre-->
            <div class="form-group">
              <label>Padre</label>
              <div align="center">
                <el-select multiple
                           filterable
                           class="sele"
                           style="height: 40%; width: 95%;"
                           size="large"
                           v-model="ParentModel"
                           placeholder="Seleccione padre">
                  <el-option v-for="option in ParentResponse"
                             class="sele"
                             :value="option.value"
                             :label="option.text"
                             :key="option.key">
                  </el-option>
                </el-select>
              </div>
            </div>
            <!--Dependencia-->
            <div class="form-group">
              <label>Dependencia</label>
              <div align="center">
                <el-select multiple
                           filterable
                           class="sele"
                           style="height: 40%; width: 95%;"
                           size="large"
                           v-model="DependenciaModel"
                           placeholder="Seleccione dependencia">
                  <el-option v-for="option in DependenciaResponse"
                             class="sele"
                             :value="option.value"
                             :label="option.text"
                             :key="option.value">
                  </el-option>
                </el-select>
              </div>
            </div>
              <!--Posicion-->
              <div class="form-group">
                <label>Posición</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="PosicionModel"
                             placeholder="Seleccione posición">
                    <el-option v-for="option in PosicionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Dedicacion-->
              <div class="form-group">
                <label>Dedicación</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="DedicacionModel"
                             placeholder="Seleccione posición">
                    <el-option v-for="option in DedicacionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Vinculacion-->
              <div class="form-group">
                <label>Vinculación</label>
                <div align="center">
                  <el-select multiple
                             filterable
                             class="sele"
                             style="height: 40%; width: 95%;"
                             size="large"
                             v-model="VinculacionModel"
                             placeholder="Seleccione vinculación">
                    <el-option v-for="option in VinculacionResponse"
                               class="sele"
                               :value="option.value"
                               :label="option.text"
                               :key="option.value">
                    </el-option>
                  </el-select>
                </div>
              </div>
              <!--Radio button-->
              <div class="form-group">
                <table>
                  <tr>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Activos">Activos</label><input type="radio" class="form-control" value="Activo" v-model="picked"></td>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Inactivos">Inactivos</label><input type="radio" class="form-control" value="Inactivo" v-model="picked"></td>
                    <td style="padding-right: 25px; padding-left: 25px"><label for="Todos">Todos</label><input type="radio" class="form-control" value="Todos" v-model="picked"></td>
                  </tr>
                </table>



              </div>
              <!--Fecha fin-->
              <div class="form-group">
                <label>Fecha fin</label>
                <div align="center">
                  <datepicker :typeable="false"
                              :bootstrap-styling="true"
                              :initialView="initialview"
                              :format="format"
                              :language="es"
                              typeable
                              placeholder="Fin"
                              :open-date="opendate"
                              v-model="fechaFin">
                  </datepicker>
                  <!--
                  <small v-if="formError.BirthDate" class="form-text text-muted text-danger">*Selecciona un valor.</small>
                  -->
                </div>
              </div>
            <!--Fecha inicio-->
            <div class="form-group">
              <label>Fecha inicio</label>
              <div align="center">
                <datepicker :typeable="false"
                            :bootstrap-styling="true"
                            :initialView="initialview"
                            :format="format"
                            :language="es"
                            typeable
                            placeholder="Inicio"
                            :open-date="opendate"
                            v-model="fechaInicio">
                </datepicker>
                <!--
                <small v-if="formError.BirthDate" class="form-text text-muted text-danger">*Selecciona un valor.</small>
                -->
              </div>
            </div>
          </div>
          <br>
          <!--Boton-->
          <div class="row">
            <button type="button" class="btn btn-lg btn-fill btn-success pull-right"  @click="check(RegionalModel,UOModel, ParentModel,DependenciaModel,PosicionModel,DedicacionModel,VinculacionModel,picked,fechaInicio,fechaFin)">
               <span class="btn-label ">
              <i class="fa el-icon-search fa-lg" style="color: white"></i>
            </span>Buscar
          </button>
            <button type="button" class="btn btn-lg btn-fill btn-warning pull-right" @click="resetForm">
             <span class="btn-label ">
              <i class="fa el-icon-delete fa-lg" style="color: white"></i>
            </span>
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  </div>
</div>
</template>
<script>
  import axios from 'axios'
  import { ModelSelect } from 'vue-search-select'
  import {DatePicker, TimeSelect, Slider, Tag, Input, Button, Select, Option} from 'element-ui'
  import Datepicker from 'vuejs-datepicker'
  import {es} from 'vuejs-datepicker/dist/locale'
  export default {
    data () {
      return {
        actiones: 'inicio',
        es: es,
        fechaFin: '',
        fechaInicio: '',
        initialview: 'day',
        format: 'yyyy/MM/dd',
        opendate: new Date(),
        picked: 'Todos',
        nada: {
          value: 0,
          label: 'Ninguno',
          key: 0
        },
        /* Formulario */
        /* Regional */
        RegionalModel: [],
        RegionalResponse: [],
        /* Unidad Organizacional */
        UOModel: [],
        UOResponse: [],
        /* Padre */
        ParentModel: [],
        ParentResponse: [],
        /* Dependencia */
        DependenciaModel: [],
        DependenciaResponse: [],
        /* Posicion */
        PosicionModel: [],
        PosicionResponse: [],
        /* Dedicacion */
        DedicacionModel: [],
        DedicacionResponse: [],
        /* Vinculacion */
        VinculacionModel: [],
        VinculacionResponse: [],
        ButtonMessage1: 'Ver trayectoria institucional',
        ButtonMessage2: 'Ver datos personales',
        insearch: false,
        action: 'SEARCH',
        query: '',
        tab: '',
        i: null,
        actions: 'LIST',
        username: '',
        url: '/BusquedaGrupal/',
        url2: '',
        propsToSearch: ['UserPrincipalName', 'Branches', 'AuthFullName', 'Dependency', 'DependencyCod', 'FullName', 'CUNI', 'Document', 'TipoLicenciaSAP', 'PersonalEmail'],
        tableColumns: [
          {
            prop: 'Id',
            label: '#',
            minWidth: 60
          },
          {
            prop: 'CUNI',
            label: 'CUNI',
            minWidth: 100
          },
          {
            prop: 'Document',
            label: 'Documento',
            minWidth: 100
          },
          {
            prop: 'FullName',
            label: 'Nombre Completo',
            minWidth: 200
          },
          {
            prop: 'Positions',
            label: 'Cargo',
            minWidth: 150
          },
          {
            prop: 'Dependency',
            label: 'Dependencia',
            minWidth: 150
          },
          {
            prop: 'Branches',
            label: 'Branches',
            minWidth: 100
          },
          {
            prop: 'Estado',
            label: 'Estado',
            minWidth: 100
          }
        ],
        pagination: {
          perPage: 5,
          currentPage: 1,
          perPageOptions: [5, 10, 20],
          total: 0
        },
        select: '',
        formData: {
          UserPrincipalName: null,
          TipoLicenciaSAP: null,
          CajaChica: false,
          SolicitanteCompras: false,
          AutorizadorCompras: false,
          PeopleId: null,
          AuthPeopleId: null,
          Rendiciones: false,
          UcbEmail: null
        }
      }
    },
    watch: {
      PeopleId: function (value) {
        if (value != null && (!this.$store.state.crud.edit)) {
          this.getDefAuth()
        }
      }
    },
    methods: {
      loadBranchesData () {
        let aux = this.RegionalResponse
        axios.get('branches/')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Name})
            })
            this.RegionalResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadOrganizationalUnitData () {
        let aux = this.UOResponse
        axios.get('BusquedaGrupal/FiltrarUO/')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Cod + '-' + element.Name, key: element.Cod + element.Id})
            })
            this.UOResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadParentData () {
        let aux = this.ParentResponse
        axios.get('BusquedaGrupal/FiltrarParent/')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Cod + '-' + element.Name, key: element.Cod + element.Id})
            })
            this.ParentResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadDependency () {
        let aux = this.DependenciaResponse
        axios.get('BusquedaGrupal/FiltrarDep/')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Cod + '-' + element.Name + '-' + element.Branch, key: element.Cod + element.Id})
            })
            this.DependenciaResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadPositionData () {
        let aux = this.PosicionResponse
        axios.get('Positions/')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Name})
            })
            this.PosicionResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadDedicationData () {
        let aux = this.DedicacionResponse
        axios.get('TableOfTables/Dedication')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Value})
            })
            this.DedicacionResponse = aux
          })
          .catch(error => console.log(error))
      },
      loadLinkageData () {
        let aux = this.VinculacionResponse
        axios.get('TableOfTables/Linkage')
          .then(response => {
            response.data.forEach(function (element) {
              aux.push({value: element.Id, text: element.Name})
            })
            this.VinculacionResponse = aux
          })
          .catch(error => console.log(error))
      },
      Modify (index) {
        console.log(index)
        this.i = index
        this.actions = 'MODIFY'
      },
      DetailPerson (index) {
        this.i = index
        console.log(index)
        this.actions = 'DETAILPERSON'
        this.action = 'DATOSPERSONA'
      },
      convert (str) {
        let date = new Date(str)
        let mnth = ('0' + (date.getMonth() + 1)).slice(-2)
        let day = ('0' + date.getDate()).slice(-2)
        return [date.getFullYear(), mnth, day].join('-')
      },
      validateForm () {
        if (this.RegionalModel.length === 0) {
          alert('Ingrese los datos requeridos')
          return false
        } else { return true }
      },
      resetForm () {
        this.RegionalModel = []
        this.UOModel = []
        this.ParentModel = []
        this.DependenciaModel = []
        this.PosicionModel = []
        this.DedicacionModel = []
        this.VinculacionModel = []
        this.picked = 'Todos'
        this.fechaInicio = ''
        this.fechaFin = ''
      },
      check (Regional, UO, Padre, Dependencia, Posicion, Dedicacion, Vinculacion, picked, fechaInicio, fechaFin) {
        if (this.validateForm()) {
          this.actiones = 'busqueda'
          this.fakeLoad()
          if (UO.length === 0) { UO = '0' }
          if (Padre.length === 0) { Padre = '0' }
          if (Dependencia.length === 0) { Dependencia = '0' }
          if (Posicion.length === 0) { Posicion = '0' }
          if (Dedicacion.length === 0) { Dedicacion = '0' }
          if (Vinculacion.length === 0) { Vinculacion = '0' }
          if (picked.length === 0) { picked = '0' }
          if (fechaInicio.length === 0) { fechaInicio = '0' } else { fechaInicio = this.convert(fechaInicio) }
          if (fechaFin.length === 0) { fechaFin = '0' } else { fechaFin = this.convert(fechaFin) }
          console.log('Mandar a axios:' + Regional + '/' + UO + '/' + Padre + '/' + Dependencia + '/' + Posicion + '/' + Dedicacion + '/' + Vinculacion + '/' + picked + '/' + fechaInicio + '/' + fechaFin)
          this.url = '/BusquedaGrupal/' + Regional + '/' + UO + '/' + Padre + '/' + Dependencia + '/' + Posicion + '/' + Dedicacion + '/' + Vinculacion + '/' + picked + '/' + fechaInicio + '/' + fechaFin
          // permite cargar la información después de haberla mandado
          this.$store.dispatch('crud/loadData', this.url)
        }
      },
      fakeLoad () {
        this.$store.commit('crud/loadSetter', true)
        setTimeout(() => {
          this.$store.commit('crud/loadSetter', false)
        }, 500)
      }
    },
    created () {
      this.loadBranchesData()
      this.loadOrganizationalUnitData()
      this.loadDependency()
      this.loadParentData()
      this.loadPositionData()
      this.loadDedicationData()
      this.loadLinkageData()
    },
    components: {
      ModelSelect,
      Datepicker,
      [DatePicker.name]: DatePicker,
      [TimeSelect.name]: TimeSelect,
      [Slider.name]: Slider,
      [Tag.name]: Tag,
      [Input.name]: Input,
      [Button.name]: Button,
      [Option.name]: Option,
      [Select.name]: Select
    }
  }
</script>
<style>
  .sele.el-select .el-input .el-input__suffix{
    display:flex;
    align-items:center
  }
  .sele.el-select .el-input:hover .el-input__icon,
  .sele.el-select .el-input:hover
  input{color:#fff}
  .sele.el-select .el-input:hover .el-input__icon::placeholder,
  .sele.el-select .el-input:hover input::placeholder{color:#fff}
  .sele.el-select .el-input:hover
  .el-input__icon,.sele.el-select .el-input:hover input{background-color: #ffffff
  }
  .sele.el-select .el-input .el-input__icon{border-radius:20px;height:40px}
  .sele.el-select .el-input input{
    background-color:#fff;
    border-color: #1c3b6c
    !important;border-width:2px;border-radius:10px;
    color: #14284b
  }
  .sele.el-select .el-input .el-input__icon
  {color: #14284b
  }
  .sele .el-tag,
  .sele .el-tag.el-tag--info
  {line-height:24px;background-color: #1c3b6c !important;border:none!important}
  .sele .el-tag.el-tag--info .el-tag__close,
  .sele .el-tag .el-tag__close
  {width:20px;height:20px;top:1px;color:#fff;background-color:transparent}
  .sele .el-tag.el-tag--info .el-tag__close:hover,
  .sele
  .el-tag .el-tag__close:hover{background-color:#fff}
</style>

