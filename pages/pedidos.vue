<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="pedidos"
      :items-per-page="10"
      :search="search"
      class="elevation-1"
      locale="pt-BR"
      light
    >
      <template #[`item.excluir`]="{ item }">
        <v-btn tile color="success" @click="excluirPedido(item)">
          <v-icon left> mdi-delete </v-icon>
          Excluir
        </v-btn>
      </template>
      <template #[`header.nome`]="{ header }">
        {{ header.text.toUpperCase() }}
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      pedidos: [],
      search: '',
      headers: [
        {
          text: 'Id pedido',
          value: 'id',
        },
        {
          text: 'Cliente',
          value: 'cliente.nome',
        },
        {
          text: 'Data',
          value: 'data',
        },
        {
          text: 'Status',
          value: 'status',
        },
        {
          text: 'Forma de Pagamento',
          value: 'formaPagamento',
        },
        {
          text: 'Excluir',
          value: 'excluir',
        },
      ],
    }
  },
  fetch() {
    this.obterPedidos()
  },
  methods: {
    async excluirPedido(pedido) {
      await this.$axios.$delete('/pedidos/' + pedido.id)
      alert('Pedido excluido')
      this.obterPedidos()
    },
    async obterPedidos() {
      this.pedidos = await this.$axios.$get('/pedidos')
    },
  },
}
</script>

<style lang="scss" scoped></style>
