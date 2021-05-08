<template>
  <v-data-table
    :headers="headers"
    dense
    :items="produtos"
    :items-per-page="10"
    no-data-text="Sem produtos cadastrados"
    class="elevation-1"
    locale="pt-BR"
    light
  >
    <template #[`top`]>
      <v-toolbar flat>
        <v-toolbar-title>Meus Produtos</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template #[`activator`]="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              Novo Produto
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <!-- botão de editar do ação -->
              <span class="headline">{{ tituloForm }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="produto.nome"
                      label="Produto"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="produto.preco"
                      label="Preço"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <!--fim do botão de ediyar do ação-->
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" text @click="fechar"> Fechar </v-btn>
              <v-btn color="blue darken-1" text @click="salvar"> Salvar </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline"
              >Tem certeza que quer deletar isto?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" text @click="fecharDialogoDeExclusao"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="confirmarExclusao"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template #[`item.acao`]="{ item }">
      <v-icon small class="mr-2" @click="editar(item)"> mdi-pencil </v-icon>
      <v-icon small @click="excluirProduto(item)"> mdi-delete </v-icon>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data() {
    return {
      produtos: [],
      dialog: false,
      dialogDelete: false,
      editedIndex: -1,
      tituloForm: 'Edição de produto',
      headers: [
        {
          text: 'Id produto',
          value: 'id',
        },
        {
          text: 'Produto',
          value: 'nome',
        },
        {
          text: 'Preço',
          value: 'preco',
        },
        {
          text: 'Ação',
          value: 'acao',
        },
      ],
      produto: {},
    }
  },
  fetch() {
    this.obterProdutos()
  },
  methods: {
    excluirProduto(produtoParaExcluir) {
      this.produto = produtoParaExcluir
      this.dialogDelete = true
    },
    async confirmarExclusao() {
      await this.$axios.$delete('/produtos/' + this.produto.id)
      this.dialogDelete = false
    },
    fecharDialogoDeExclusao() {
      this.produto = {}
      this.dialogDelete = false
    },
    async editar(produtoParaEditar) {
      this.produto = await this.$axios.$get('/produtos/' + produtoParaEditar.id)
      this.dialog = true
    },
    async obterProdutos() {
      this.produtos = await this.$axios.$get('/produtos')
    },
    fechar() {
      this.dialog = false
    },
    async salvar() {
      await this.$axios.$put('/produtos/' + this.produto.id, this.produto)
      this.dialog = false
      this.obterProdutos()
    },
  },
}
</script>

<style lang="scss" scoped></style>
