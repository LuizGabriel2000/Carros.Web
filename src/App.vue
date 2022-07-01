<template>
  <v-app>
    <v-main>
      <HomeCarrosVue/>
    </v-main>
  </v-app>
</template>

<script>

import HomeCarrosVue from './views/HomeCarros.vue';
export default {
  name: 'App',

  components: {
    HomeCarrosVue,
  },

  data: () => ({
    //
  }),
};
</script>
