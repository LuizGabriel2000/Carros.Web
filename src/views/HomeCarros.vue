<template>
<div style="height: 100%">
    <div class="img-fundo">
        <NavBarVue/>
        <v-container>
            <v-card  class="my-10" color="rgba(0, 0, 0, 0.220)">
                <v-row>
                    <v-col cols="3">
                        <h2 class="ma-4" style="color: beige;">Adicionar Carro:</h2>
                    </v-col>

                    <v-col cols="3">
                        <v-text-field v-model="formData.modelo" :rules="rules"></v-text-field>
                            <p style="color: maroon;">Marca - Modelo (Exemplo: "Fiat Uno")</p>
                    </v-col>

                    <v-col cols="3">
                         <v-text-field v-model="formData.placa" :rules="rules"></v-text-field>
                          <p style="color: maroon;" >Placa (Exemplo: "HSP2367")</p>
                    </v-col>

                    
                    <v-col cols="3">
                         <v-btn class="ml-15 mt-10" color="primary" @click="Adicionar()">Adicionar</v-btn>
                    </v-col>
                </v-row>
            </v-card>

                    

            <v-card class="my-15" color="rgba(0, 0, 0, 0.290)">
                <v-row>
                    <v-col cols="4">
                        <h2 style="color: beige;" class="ma-4">Lista dos Carros:</h2>
                    </v-col>

                    <v-col cols="8">
                        <v-row>
                            <v-col cols="5">
                                <h3 style="color: blue;">Marca e Modelo</h3>
                            </v-col>

                            <v-col cols="7">
                                <h3 style="color: blue;">Placa</h3>
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
                                        <v-col cols="5">
                                            <h3>{{item.descricao}}</h3>
                                        </v-col>

                                        <v-col cols="3">
                                            
                                            <div id="placa-carro" class="placa-carro">
                                                <div class="targeta">BRASIL</div>
                                                {{item.placa}}
                                            </div>
                                        </v-col>

                                        <v-col cols="4">
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
        benched: 0, 
        listaCarro: [],
        formData: {
            modelo: null,
            placa:null
        },
        modeloCarro: '',
        placaCarro: ''
    }
  },

  mounted () {
    this.BuscarCarros()
  },

  methods: {
    Adicionar() {
        axios
        .post(`http://localhost:3000/api/carro`, this.formData)
        .then(response => {
            console.log(response)
            this.BuscarCarros()
            this.formData.modelo = null
            this.formData.placa = null
        })
    },

    BuscarCarros() {
        axios
        .get(`http://localhost:3000/api/carros`)
        .then(response => (this.listaCarro = response.data.result))
    },

    DeletarCarro(codigo) {
        axios.delete(`http://localhost:3000/api/carro/${codigo}`).then((res) => {
            console.log("apagou com secesso", res.data)
            this.BuscarCarros()
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

