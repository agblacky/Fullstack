<template>
  <v-app>
    <LogoBar />
    <v-main>
      <router-view :cars="cars" @getCars="getCars"></router-view>
    </v-main>
  </v-app>
</template>

<script>
import LogoBar from '@/components/LogoBar.vue';
import axios from 'axios';
export default {
  name: 'App',
  components: {
    LogoBar,
  },

  data: () => ({
    cars: null,
  }),
  created() {
    this.getCars();
  },
  methods: {
    async getCars() {
      const { data } = await axios({
        url: 'http://localhost:3000/cars',
        method: 'GET',
      });
      this.cars = data.data;
    },
  },
};
</script>
