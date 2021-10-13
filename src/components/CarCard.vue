<template>
  <v-card max-width="374" class="d-inline mx-4 my-4">
    <v-img height="250" :src="car.image"></v-img>

    <v-card-title>{{ car.title }} <span v-if="IsReserved()"> *RESERVED*</span></v-card-title>

    <v-card-text>
      <div>Owner: {{ car | namebuilder }}</div>
      <div class="my-4">
        Year: {{ car.year_of_make }}<br />
        Miles: {{ car.miles }}
      </div>
      <div class="my-4">Price: {{ car | priceCheck }}</div>
    </v-card-text>

    <v-card-actions class="ma-2">
      <v-btn depressed class="red darken-2 white--text" @click="deleteCar">
        Delete
      </v-btn>
      <v-spacer></v-spacer>
      <router-link :to="`/details/${car.id}`" v-if="!IsReserved()">
        <v-btn depressed class="purple darken-2 white--text">
          Details
        </v-btn>
      </router-link>
    </v-card-actions>
  </v-card>
</template>

<script>
import axios from 'axios';
export default {
  props: {
    car: Object,
  },
  filters: {
    namebuilder: function(value) {
      return value.first_name + ' ' + value.last_name;
    },
    priceCheck: function(value) {
      if (value.status === 'reserved') {
        return 'N/A';
      }
      return value.price;
    },
  },
  methods: {
    IsReserved() {
      return this.car.status == 'reserved';
    },
    async deleteCar() {
      await axios({
        url: 'http://localhost:3000/cars/' + this.car.id,
        method: 'DELETE',
      });
      await this.$emit('getCars');
    },
  },
};
</script>

<style lang="scss" scoped></style>
