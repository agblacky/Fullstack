<template>
  <div>
    <CarCards :cars="cars" @getCars="$emit('getCars')"/>
  </div>
</template>

<script>
import CarCards from '@/components/CarCards.vue';
export default {
  name: 'Home',
  components: {
    CarCards,
  },
  props: {
    cars: Array,
  },
};
</script>
