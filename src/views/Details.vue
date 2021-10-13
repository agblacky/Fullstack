<template>
  <v-container>
    <v-card class="mx-auto" max-width="500">
      <img :src="this.getCar().image" style="width:100%" />

      <v-container>
        <p class="text-center text-h5 font-weight-bold">
          {{ this.getCar().title }}<span v-if="IsReserved()"> *RESERVED*</span>
        </p>
        <v-simple-table>
          <thead>
            <tr>
              <th class="text-left">
                Price
              </th>
              <th class="text-left">
                Miles
              </th>
              <th class="text-left">
                Year of Make
              </th>
              <th class="text-left">
                Current Owner
              </th>
            </tr>
          </thead>
          <thead>
            <tr>
              <td class="text-left py-2">{{ this.getCar().price }}</td>
              <td class="text-left py-2">{{ this.getCar().miles }}</td>
              <td class="text-left py-2">{{ this.getCar().year_of_make }}</td>
              <td class="text-left py-2">
                {{ this.getCar() | namebuilder }}
              </td>
            </tr>
          </thead>
        </v-simple-table>
        <v-card-text>
          {{ this.getCar().description }}
        </v-card-text>

        <v-card-actions>
          <router-link to="/">
            <v-btn color="purple darken-2 white--text">
              Go back
            </v-btn>
          </router-link>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-3 white--text"
            @click="reserve()"
            v-if="!IsReserved()"
          >
            order car
          </v-btn>
        </v-card-actions>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  props: {
    id: {
      type: String,
    },
    cars: {
      type: Array,
    },
  },
  data() {
    return {
      getCar: function() {
        return this.cars.find((el) => el.id == this.id);
      },
    };
  },
  created() {
    if (!this.cars) {
      this.$router.push('/');
    }
  },
  methods: {
    async reserve() {
      await axios({
        url: 'http://localhost:3000/cars/' + this.id,
        method: 'PATCH',
        contentType: 'application/json',
        data: {
          status: 'reserved',
        },
      });
      await this.$emit('getCars');
    },
    IsReserved() {
      return this.getCar().status == 'reserved';
    },
  },
  filters: {
    namebuilder: function(value) {
      return value.first_name + ' ' + value.last_name;
    },
  },
};
</script>

<style lang="scss" scoped></style>
