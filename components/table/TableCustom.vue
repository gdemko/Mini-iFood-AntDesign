<template>
  <div>
    <div class="text-center" v-if="loading">
      <a-spin style="width: 3rem; height: 3rem;" label="Loading..."></a-spin>
    </div>
    <div v-else>
      <a-card>
        <a-row class="mb-4">
          <a-col align-self="start">
            <slot name="header"></slot>
            <span class="h2">{{title}}</span>
          </a-col>
        </a-row>
        <table responsive="sm">
          <thead>
            <tr>
              <th v-for="(field, index) in fields" :width="field.width" :key="index" >{{field.label}}</th>
              <th v-if="!removeOption" class="text-center" >Opções</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="items.length == 0" >
              <th align="center" :colspan="fields.length+1">Nenhum resultado encontrado</th>
            </tr>
            <tr v-else v-for="(item, index) in items" :key="index" >
              <th v-for="(it, index) in item" :key="index" >
                {{it}}
              </th>
              <th v-if="customAction" class="text-center">
                <slot v-bind:index="index" name="action"></slot>
              </th>
              <th class="text-center" v-else>
                <a-button-group>
                  <a-button v-if="show" :to="route +`/`+item.codigo" >Visualizar</a-button>
                  <a-button v-if="edit" :to="route +`/edit/`+item.codigo" variant="primary">Editar</a-button>
                  <a-button v-if="remove" @click="destroy(item.codigo)" variant="danger">Excluir</a-button>
                </a-button-group>
              </th>
            </tr>
          </tbody>
        </table>
        <a-button v-if="buttomAdd" :to="routeAdd" variant="success">Adicionar</a-button>
        <a-row v-else>
          <a-col span="24">
            <slot name="footer"></slot>
          </a-col>
        </a-row>
      </a-card>
    </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'

  export default {
    data() {
      return {
        loading: true
      }
    },
    mounted() {
      this.loader()
    },
    props:{
      fields:{
        type: Array,
        required: true
      },
      items:{
        type: Array,
        required: true
      },
      title: {
        type: String,
        required: true
      },
      route:{
        type: String,
      },
      routeAdd: {
        type: String,
      },
      customAction:{
        type: Boolean,
        default: false
      },
      show: {
        type:Boolean,
        default: false
      },
      edit: {
        type:Boolean,
        default: false
      },
      remove: {
        type:Boolean,
        default: false
      },
      buttomAdd: {
        type:Boolean,
        default: false
      },
      removeOption: {
        type:Boolean,
        default: false
      }
    },
    methods:{
      loader(){
        this.loading = false
      },
      destroy(id) {
        Swal.fire({
          title: 'Deseja deletar?',
          text: "Se você deletar não tera como recuperar!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Sim, deletar!',
          cancelButtonText: 'Cancelar'
        }).then((result) => {
          if (result.isConfirmed) {
            this.$axios
            .$delete(
              this.route + `/` + id
            )
            .then(({success, data, message, error_message}) => {
              if(success == true)
              {
                Swal.fire(
                  'Deletado!',
                  message,
                  'success'
                ).then(() => {
                  document.location.reload(true);
                })
              } else {
                this.message = message
                this.error_message = error_message
              }
            })
          }
        })
      }
    }
  }
</script>
