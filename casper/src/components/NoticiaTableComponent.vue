<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :expanded.sync="expanded"
    item-key="name"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>ADM</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              Nova Notícia
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="6"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.titulo"
                      label="Titulo da noticia"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="6"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.tema"
                      label="Tema"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.descricao"
                      label="Descrição"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.link"
                      label="Link"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.imglink"
                      label="Imagem Link"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
     <template v-slot:expanded-item="{ headers, item }">
      <td :colspan="headers.length">
      <br/>
        Tema: {{ item.tema }}
        <br/>
        Título: {{ item.titulo }}
        <br />
        Descrição: {{ item.descricao }}
        <br/>
        <a :href="item.link" target="_blank">Link da reportagem</a>
        <br/>
        <a :href="item.link" target="_blank">Link da imagem</a>
        <br/>
      </td>
    </template>
    <!-- eslint-disable-next-line -->
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
    data: () => ({
      expanded: [],
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'Notícias',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Tema', value: 'tema', sortable: false},
        { text: 'Título', value: 'titulo', sortable: false},
        { text: '', value: 'data-table-expand' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        tema: '',
        titulo: '',
        descricao:'',
        link:'',
        imglink: '',
      },
      defaultItem: {
        tema: '',
        titulo: '',
        descricao: '',
        link:'',
        imglink: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.desserts = [
          {
            tema: 'Esportes',
            titulo: "Felipão sinaliza ficar como técnico do Athletico até o fim de 2022: 'Não era o posicionamento inicial'",
            descricao: "Luiz Felipe Scolari diz que pretendia ser apenas diretor, mas deve ficar na beira do gramado no restante da temporada. Felipão estreia no Athletico diante do Tocantinópolis na terça-feira",
            link: "https://ge.globo.com/pr/futebol/times/athletico-pr/noticia/2022/05/09/dupla-funcao-felipao-tecnico-athletico-pr-fim-2022-nao-era-posicionamento-inicial.ghtml",
            imglink: "https://s2.glbimg.com/W8kQIChNo4eF8ULviNqzvv1ZMgM=/0x0:624x367/984x0/smart/filters:strip_icc()/i.s3.glbimg.com/v1/AUTH_bc8228b6673f488aa253bbcb03c80ec5/internal_photos/bs/2022/Z/v/HQmHufSsajT41t1UkK0A/felipao-2-1-.jpg",
          },
          {
            tema: 'Entretenimeto',
            titulo: 'Rock in Rio 2022: ingressos, shows, datas e mais!',
            descricao: 'Começa hoje (5), às 19h, a venda dos ingressos para o Rock in Rio 2022, que será realizado nos dias 2, 3, 4, 8, 9, 10 e 11 de setembro',
            link: "https://www.tecmundo.com.br/minha-serie/236606-rock-in-rio-2022-ingressos-shows-horarios.htm",
            imglink: 'https://tm.ibxk.com.br/2022/04/05/05171529694455.jpg?ims=704x264'
          },
          {
            tema: 'Famosos',
            titulo: 'Parque temático da Turma da Mônica em Gramado será inaugurado em outubro',
            descricao: 'Depois de parque de diversões em SP, musicais e muitos brinquedos, a Turma da Mônica terá um parque temático em Gramado (RS)',
            link: 'https://revistaquem.globo.com/viagem-e-comida/noticia/2022/05/parque-tematico-da-turma-da-monica-em-gramado-sera-inaugurado-em-outubro.html',
            imglink: 'https://s2.glbimg.com/udvfh9yoiHnCyxiULsB2TBgBLFc=/620x413/top/e.glbimg.com/og/ed/f/original/2022/05/05/vila-da-monica-gramado---divulgacao_1.jpg'
          },
        ]
      },

      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        this.desserts.splice(this.editedIndex, 1)
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>
