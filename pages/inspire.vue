<template>
  <div>
    <h1>nome do cliente - {{ cliente.nome }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cliente: { nome: '' },
    }
  },
  created() {
    this.cliente = this.$axios.$get('/clientes/1')
  },
}
</script>

<style lang="scss" scoped></style>
