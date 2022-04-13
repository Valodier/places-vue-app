<template>
  <div class="home">
    <h1>Look at all the Places!</h1>
    <div>
      <h4>Create a place.</h4>
      Name:
      <input type="text" v-model="newPlacesParams.name" />
      Address:
      <input type="text" v-model="newPlacesParams.address" />
      <br />
      <button v-on:click="placesCreate()">BRING FORTH</button>
    </div>
    <div v-for="place in places" :key="place.id">
      <h2>{{ place.name }}</h2>
      <h3>{{ place.address }}</h3>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
      newPlacesParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/places").then((response) => {
      this.places = response.data;
      console.log("Places Index", this.places);
    });
  },
  methods: {
    placesCreate: function () {
      axios
        .post("/places", this.newPlacesParams)
        .then((response) => {
          console.log("Good news!", response.data);
          this.places.push(response.data);
          this.newPlacesParams = {};
        })
        .catch((error) => {
          this.error = error.response.data.errors;
        });
    },
  },
};
</script>

<style></style>
