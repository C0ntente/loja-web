<template>
  <div>
    <h1>nome do cliente - {{ cliente.nome }}</h1>
    <h2>cpf do cliente - {{ cliente.cpf }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cliente: {},
    }
  },
  async fetch() {
    this.cliente = await this.$axios.$get('/clientes/1')
  },
}
</script>

<style lang="scss" scoped></style>
