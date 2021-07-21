<template>
  <div class="home">
    <h1>New Place</h1>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Address:
      <input type="text" v-model="newPlaceParams.price" />
    </div>
    <button v-on:click="createPlace()">Create</button>
    <h1>{{ message }}</h1>
    <p>{{ message1 }}</p>
    <div v-for="place in places" v-bind:key="place.id">
      <h1>{{ place.name }}</h1>
      <h2>{{ place.address }}</h2>
      <button v-on:click="showPlace(Place)">More Info!</button>
    </div>
    <dialog id="place-details">
     <form method="dialog">
        <h1>Place Info!</h1>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update Place!</button>
        <button v-on:click="deletePlace(currentPlace)">Destroy Place!</button>
        <button>Close</button>
      </form>
    </dialog>
    <ul>
      <li v-for="error in errors" :key="error.id">{{ error }}</li>
    </ul>  
  </div>
</template>

<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: {},
      newPlaceParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlacess: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        this.places = response.data;
        console.log("All Places:", this.places);
      });
    },
    createPlace: function () {
      console.log("Place Added");
       axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
      },
      showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
      },
      
      updatePlace: function (place) {
      var editPlaceParams = place;
      axios.patch("http://localhost:3000/places/" + place.id, editPlaceParams).then((response) => {
        console.log("Success!", response.data);
        });
      },
      deletePlace: function (place) {
        axios.delete("http://localhost:3000/places/" + place.id).then((response) => {
          console.log("Success!", response.data);
          var index = this.places.indexOf(response);
          this.places.splice(index, 1);
      });
    },
  },
};
</script>
