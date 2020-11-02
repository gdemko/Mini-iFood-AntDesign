<template>
  <a-row>
    <a-col>
      <loader v-if="loading" />
      <order-form @change="submit" title="Adicionar Pedido" :error-message="error_message"></order-form>
    </a-col>
  </a-row>
</template>

<script>
import Swal from 'sweetalert2'

export default {
  data() {
    return {
      error_message:"",
      loading:false
    }
  },
  methods: {
    submit(form){
      console.log(form)
      this.$axios
        .$post(
          'ordered', form
        )
        .then(({success, message, data}) => {
          console.log('===>>>', data)
          if(success == true)
          {
            Swal.fire(
              'Registrado!',
              message,
              'success'
            ).then(() => {
                this.$router.push(`/pedido-finalizado/${data.id}`)
            })
          }
        }).catch((error) => {
            this.error_message = error.response.data.error_message;
            console.error(this.error_message)
            console.error(error)
            Swal.fire(
              'Erro!',
              error.response.data.message,
              'error'
            )
        }).finally(()=>{
          this.$nuxt.$loading.finish()
        })
    }
  },
}
</script>
