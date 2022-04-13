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
      <button v-on:click="placesCreate()">SUMMON</button>
    </div>
    <div v-for="place in places" :key="place.id">
      <h2>{{ place.id }}</h2>
      <button v-on:click="placesShow(place)">MORE</button>
      <br />
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <p>Name: {{ currentPlace.name }}</p>
        <input type="text" v-model="currentPlace.name" />
        <p>Address: {{ currentPlace.address }}</p>
        <input type="text" v-model="currentPlace.address" />
        <br />
        <button v-on:click="placesUpdate(currentPlace)">Edit</button>
        <br />
        <button v-on:click="placesDestrot(currentPlace)">UNMAKE</button>
        <br />
        <button>RELEASE</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
      currentPlace: {},
      newPlacesParams: {},
      errors: [],
    };
  },
  created: function () {
    this.placesIndex();
  },
  methods: {
    placesIndex: function () {
      axios.get("/places").then((response) => {
        this.places = response.data;
        console.log("Places Index", this.places);
      });
    },
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
    placesShow: function (place) {
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    placesUpdate: function (place) {
      var editPlaceParams = place;
      axios
        .patch("/places/" + place.id, editPlaceParams)
        .then((response) => {
          console.log("Update successful!", response.data);
          this.currentPlace = {};
        })
        .catch((error) => {
          console.log((this.error = error.response.data.errors));
        });
    },
    placesDestroy: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log("UNEXISTED SUCCESSFULLY", response.data);
        var index = this.place.indexOf(place);
        this.place.splice(index, 1);
      });
    },
  },
};
</script>

<style></style>
