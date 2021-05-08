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
      :items="clientes"
      :items-per-page="10"
      :search="search"
      class="elevation-1"
      locale="pt-BR"
      light
    >
      <template #[`item.excluir`]="{ item }">
        <v-btn tile color="success" @click="excluirCliente(item)">
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
      clientes: [],
      search: '',
      headers: [
        {
          text: 'Cliente id',
          value: 'id',
        },
        {
          text: 'Cliente',
          value: 'nome',
        },
        {
          text: 'Cpf',
          value: 'cpf',
        },
        {
          text: 'E-mail',
          value: 'email',
        },
        {
          text: 'Excluir',
          value: 'excluir',
        },
      ],
    }
  },
  fetch() {
    this.obterClientes()
  },
  methods: {
    async excluirCliente(cliente) {
      await this.$axios.$delete('/clientes/' + cliente.id)
      alert('Cliente excluido')
      this.obterClientes()
    },
    async obterClientes() {
      this.clientes = await this.$axios.$get('/clientes')
    },
  },
}
</script>

<style lang="scss" scoped></style>
