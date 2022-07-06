<template>
<div style="height: 100%">
    <div class="img-fundo">
        <NavBarVue :buscar="(pesquisa) => buscar(pesquisa)"/>
        <v-container>
            <v-card  class="my-10" color="rgba(0, 0, 0, 0.220)">
                <v-row>
                    <v-col cols="2">
                        <h2 class="ma-4" style="color: beige;">Adicionar Carro:</h2>
                    </v-col>

                    <v-col cols="2">
                        <v-text-field v-model="formData.marca"></v-text-field>
                            <p style="color: maroon;">Marca (Exemplo:"Fiat")</p>
                    </v-col>

                    <v-col cols="2">
                         <v-text-field v-model="formData.modelo"></v-text-field>
                          <p style="color: maroon;" >Modelo (Exemplo:"Uno")</p>
                    </v-col>

                    <v-col cols="2">
                        <v-select
                            v-model="formData.cor"
                            :items="coresCarros"
                            item-text="texto"
                            item-value="cor"
                            label="Selecione as cores"
                            :menu-props="{ botton: true, offsetY: true }"
                        ></v-select>
                            <p style="color: maroon;">Cor - (Exemplo: "Azul")</p>
                    </v-col>


                    <v-col cols="2">
                        <v-text-field v-model="formData.placa"></v-text-field>
                            <p style="color: maroon;">Placa (Exemplo:"BIU2J19")</p>
                    </v-col>

                    
                    

                    
                    <v-col cols="2">
                         <v-btn class="ml-7 mt-15" color="primary" @click="Adicionar()">Adicionar</v-btn>
                    </v-col>
                </v-row>
            </v-card>

                    

            <v-card class="my-15" color="rgba(0, 0, 0, 0.290)">
                <v-row>
                    <v-col cols="3">
                        <h2 style="color: beige;" class="ma-4">Lista dos Carros:</h2>
                        <p class="ml-2" style="color:aqua">A Api está disponivel no GitHub</p>
                    </v-col>

                    <v-col cols="9">
                        <v-row>
                            <v-col cols="3">
                                <h3 style="color: blue;">Marca</h3>
                            </v-col>

                            <v-col cols="3">
                                <h3 style="color: blue;">Modelo</h3>
                            </v-col>

                            <v-col cols="2">
                                <h3 style="color: blue;">Cor</h3> 
                            </v-col>

                            <v-col cols="2">
                                <h3 style="color: blue;">Placa</h3>
                            </v-col>

                            <v-col cols="2">
                            </v-col>
                        </v-row>
                        
                        <v-virtual-scroll
                            :bench="benched"
                            :items="listaCarro"
                            :height="(listaCarro.length * 60) > 360 ? 360 : (listaCarro.length * 60)"
                            item-height="60"
                        >
                            <template v-slot:default="{ item }">
                                <v-list-item :key="item">
                                    <v-row>
                                        <v-col cols="3">
                                            <h3>{{item.marca}}</h3>
                                        </v-col>

                                        <v-col cols="3">
                                            <h3>{{item.descricao}}</h3>
                                        </v-col>

                                        <v-col cols="2">
                                            <v-icon :color="item.cor">fa-solid fa-car</v-icon>
                                        </v-col>



                                        <v-col cols="2">                                       
                                            <div id="placa-carro" class="placa-carro">
                                                <div class="targeta">BRASIL</div>
                                                {{item.placa}}
                                            </div>
                                        </v-col>

                                        <v-col cols="1">
                                            <v-btn
                                                color="#fff"
                                                depressed
                                                icon
                                                @click="EditarCarro(item)"
                                            >
                                            <v-icon color="primary">mdi-wrench</v-icon> 

                                            </v-btn>

                                            

                                        </v-col>



                                         <v-col cols="1">
                                            <v-btn
                                                color="#fff"
                                                depressed
                                                icon
                                                @click="DeletarCarro(item.codigo)"
                                            >
                                            <v-icon color="red">mdi-close</v-icon> 

                                            </v-btn>

                                            

                                        </v-col>

                                        
                                    </v-row>
                                </v-list-item>
                                <v-divider></v-divider>
                            </template>
                        </v-virtual-scroll>

                    </v-col>

                </v-row>
            </v-card>
        </v-container>
      
    </div>
    <v-dialog
      v-model="dialog"
      persistent
      max-width="480"
    >
      <v-card>
        <v-card-title class="text-h5">
          Editar Carro
        </v-card-title>

        <v-card-text class="d-flex">
            <div class="mx-3">
                <v-text-field v-model="formEdit.marca" label="Marca"></v-text-field>
                <v-text-field v-model="formEdit.modelo" label="Modelo"></v-text-field>
            </div>
            <div class="mx-3">
                <v-select
                    v-model="formEdit.cor"
                    :items="coresCarros"
                    item-text="texto"
                    item-value="cor"
                    :menu-props="{ botton: true, offsetY: true }"
                ></v-select>
                <v-text-field v-model="formEdit.placa" label="Placa"></v-text-field>
            </div>
        </v-card-text>

        <v-card-actions>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            Fechar
          </v-btn>

          <v-spacer></v-spacer>
          
          <v-btn
            color="primary"
            text
            @click="Salvar()"
          >
            Salvar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

</div>
 
</template>

<script>
import axios from "axios"

import NavBarVue from '@/components/commons/NavBar.vue'

export default {
 components: {
    NavBarVue,
},
  data() {
    return { 
        ListaCores: [
            "Preto",
            "Branco",
            "Azul",
            "Amarelo",
            "Vermelho",
            "Rosa",
            "Prata",
            "Marron",
            "Verde"
        ],  
        coresCarros: [], 
        dialog: false,
        ModalCor: false,
        benched: 0, 
        lista: [],
        listaCarro: [],
        formData: {
            codigo: null,
            marca: null,
            modelo: null,
            placa: null,
            cor: null
        },
        formEdit: {
            codigo: null,
            marca: null,
            modelo: null,
            placa: null,
            cor: null
        },
        modeloCarro: '',
        placaCarro: '',
    }
  },

  mounted () {
    this.BuscarCarros()
    this.corCarro()

  },

  methods: {
    Adicionar() {

        axios
        .post(`http://localhost:3000/api/carro`, this.formData)
        .then(response => {
            console.log(response)
            this.BuscarCarros()
            this.formData.marca = null
            this.formData.modelo = null
            this.formData.placa = null
        })
    },

    BuscarCarros() {
        axios
        .get(`http://localhost:3000/api/carros`)
        .then(response => {
            this.lista = response.data.result
            this.listaCarro = response.data.result
        })
    },

    DeletarCarro(codigo) {
        axios.delete(`http://localhost:3000/api/carro/${codigo}`)
        .then((res) => {
            console.log("Removido com sucesso", res.data)
            this.BuscarCarros()
        })
    },

    EditarCarro(item) {
        console.log('caralhoooooooooooooooooo', item.cor)

        this.formEdit.cor = item.cor
        this.formEdit.marca = item.marca
        this.formEdit.codigo = item.codigo
        this.formEdit.modelo = item.descricao
        this.formEdit.placa = item.placa
        this.dialog = true
        

    },

    Salvar() {
        axios.put(`http://localhost:3000/api/carro/${this.formEdit.codigo}`, this.formEdit)
        .then((res) => {
            console.log("Editado com secesso", res.data)
            this.BuscarCarros()
            this.dialog = false
        })
    },

    buscar(pesquisa) {

        let novaLista = this.lista.reduce((lista, item) => {
            let modelo = item.descricao.match(new RegExp(pesquisa, "gi"))
            let placa = item.placa.match(new RegExp(pesquisa, "gi"))

            if (modelo || placa) {
                lista.push(item)
            }

            return lista
        }, [])


        if (pesquisa) {
            this.listaCarro = novaLista
        } else {
            this.listaCarro = this.lista
        }

    },

    corCarro() {
        axios
        .get(`http://localhost:3000/api/cor-carro`)
        .then(response => {
            console.log('aqui', response)
            this.coresCarros = response.data.result
        })
    }

  }
}
</script>

<style lang="scss">
.img-fundo {
    background-image: url('https://auonline.com.br/uploads/redactor/noticias/fotos/201908//b38f366e0cdf9be2fe5b8f273f6b7b98.jpeg');
    width: 100%;
    height: 100%;
};

#placa-carro {
    background-color: rgb(250, 240, 240);
    max-width: 110px;
    text-align: center;
    font-weight: 600;

    .targeta {
        background-color: blue;
        font-size: 8px;
        color: white;
    }
}

</style>

