<template>
  <div>
        <v-app-bar
            color="deep-purple accent-4"
            dense
            dark
        >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Controle De Carros</v-toolbar-title>

      <v-spacer></v-spacer>
      <div class="d-flex" style="width: 400px">
        <v-text-field @input="buscar(pesquisa)" v-model="pesquisa" label="Buscar veiculo"></v-text-field>
        <v-btn @click="buscar(pesquisa)" icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </div>
    </v-app-bar>
    </div>
</template>

<script>
export default {
  props: {
    buscar: Function
  },
  data() {
    return {
      pesquisa: ''
    }
  }
}
</script>

<style>
</style>