<template>
  <div>
    <h1>{{title}}</h1>
    <a-card>
      <slot></slot>

      <a-button v-if="footer" variant="success" type="submit">Salvar</a-button>
      <a-button v-if="footer" @click="clear()" variant="primary">Limpar</a-button>
      <a-button :to="routeBack">Voltar</a-button>

    </a-card>
  </div>
</template>

<script>
export default {
  props:{
    title: {
      type: String,
      required: true
    },
    routeBack: {
      type: String
    },
    footer: {
      default: false,
      type: Boolean
    }
  },
  methods:{
    clear(){
      this.$emit("clear", true)
    }
  }
}
</script>
