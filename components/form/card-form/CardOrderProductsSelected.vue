<template>
  <a-card>
    <slot></slot>
    <table-custom :fields="fields" :items="data" title="Produtos Selecionados" customAction>
      <template v-slot:action="slotProps">
        <a-button variant="danger" @click="destroy(slotProps.index)"> Excluir</a-button>
      </template>
      <template v-slot:footer>
        <slot name="footer"></slot>
      </template>
    </table-custom>
    <slot  name="description"></slot>
  </a-card>
</template>

<script>
export default {
  props:{
    data:{
      type: Array,
      required:true
    }
  },
  data() {
    return {
      fields: [
        { key: 'codigo', label: 'Código', width: '10%' },
        { key: 'name', label: 'Nome Produto', width: '30%' },
        { key: 'quantity', label: 'Quantidade', width: '10%' },
        { key: 'value', label: 'Preço', width: '10%' },
        { key: 'sub_total', label: 'Subtotal', width: '30%' }
      ],
    }
  },
  methods:{
    destroy(id){
      this.$emit('remove', id)
    }
  }
}
</script>
