<template>
  <v-container class="d-flex justify-space-around align-stretch my-4 flex-wrap">
    <CarCard :car="car" v-for="car in cars" :key="car.id" @getCars="getCars" />
  </v-container>
</template>

<script>
import CarCard from '@/components/CarCard.vue';

export default {
  components: {
    CarCard,
  },
  props: {
    cars: Array,
  },
  methods: {
    getCars() {
      this.$emit('getCars');
    },
  },
};
</script>

<style lang="scss" scoped></style>
