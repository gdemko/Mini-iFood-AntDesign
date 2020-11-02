<template>
    <div>
      <a-row class="text-center mb-1">
          <a-col>
              <a-card color="primary" block bg-variant="success" text-variant="white" class="text-center" span="24" md="auto">
                  <span>{{user.name}} seu pedido foi registrado com Sucesso!! Tempo médio para retirada, 15 a 30 minutos. </span>
              </a-card>
          </a-col>
      </a-row>
      <a-row class="text-center mb-1">
          <a-col>
              <a-card color="primary" block bg-variant="danger" text-variant="white" class="text-center">
                  <span>Número do Pedido: {{this.$route.params.index}}</span>
              </a-card>
          </a-col>
      </a-row>
      <a-row class="text-center mb-1">
          <a-col>
              <a-card color="primary" block bg-variant="info" text-variant="white" class="text-center">
                  <span>Agradecemos pela sua preferência, bom apetite e não esqueça de nos mencionar nas redes sociais. ^-^</span>
              </a-card>
          </a-col>
      </a-row>
    </div>
</template>

<script>
export default {
    data() {
    return {
      user: this.$auth.user
    }
  },
}
</script>
