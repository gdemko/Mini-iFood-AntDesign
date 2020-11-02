<template>
  <a-card>
      <a-row>
        <a-col span="14" class="font-weight-bold">
          Produto
        </a-col>
        <a-col span="10" class="font-weight-bold">
          Quantidade
        </a-col>
      </a-row>
    <slot></slot>
    <slot name="footer"></slot>
  </a-card>
</template>

<script>
export default {

}
</script>
