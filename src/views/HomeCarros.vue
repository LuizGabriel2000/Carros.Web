<template>
<div>
    <div class="img-fundo">
        <NavBarVue/>
        <div class="pa-16">
            <v-card  class="my-10" color="rgba(0, 0, 0, 0.220)">
                <v-row>
                    <v-col cols="3">
                        <h2 class="ma-4" style="color: beige;">Adicionar Carro:</h2>
                    </v-col>

                    <v-col cols="3">
                        <v-text-field :rules="rules"></v-text-field>
                            <p style="color: maroon;">Marca - Modelo (Exemplo: "Fiat Uno")</p>
                    </v-col>

                    <v-col cols="3">
                         <v-text-field :rules="rules"></v-text-field>
                          <p style="color: maroon;" >Placa (Exemplo: "HSP2367")</p>
                    </v-col>

                    
                    <v-col cols="3">
                         <v-btn class="ml-15 mt-10" color="primary">Adicionar</v-btn>
                    </v-col>
                </v-row>
            </v-card>

                    

            <v-card class="my-15" color="rgba(0, 0, 0, 0.290)">
                <v-row>
                    <v-col cols="4">
                        <h2 style="color: beige;" class="ma-4">Lista dos Carros:</h2>
                    </v-col>

                    <v-col cols="4">
                        <h3 style="color: blue;">Marca e Modelo</h3>
                    </v-col>

                    <v-col cols="4">
                        <h3 style="color: blue;">Placa</h3>
                    </v-col>

                    <v-col cols="4"></v-col>

                    <v-col cols="4">
                      <strong>{{carro}}</strong>
                    </v-col>

                    <v-col cols="4">
                        
                        <div id="placa-carro" class="placa-carro">
                            <div class="targeta">BRASIL</div>
                            {{placa}}
                        </div>
                    </v-col>
                </v-row>
            </v-card>
        </div>
      
    </div>
    
</div>
 
</template>

<script>
import NavBarVue from '@/components/commons/NavBar.vue'
export default {
 components: {
    NavBarVue,
  },
  data() {
    return {
        carro: "BMW 320i",
        placa: "BMW1I93",
        info: []
    }
  },

    mounted () {
    axios
      .get('')
      .then(response => (this.info = response))
  },

  methods: {
    
  }
}
</script>

<style lang="scss">
.img-fundo {
    background-image: url('https://auonline.com.br/uploads/redactor/noticias/fotos/201908//b38f366e0cdf9be2fe5b8f273f6b7b98.jpeg');
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
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

