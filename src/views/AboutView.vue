
<script>
import mapboxgl from 'mapbox-gl'; // or "const mapboxgl = require('mapbox-gl');"
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
    };
  },
  mounted: function () {
    this.makeMap();
  },
  methods: {
    makeMap: function () {
      mapboxgl.accessToken = process.env.VUE_APP_MAP_KEY;
      const map = new mapboxgl.Map({
        container: 'map', // container ID
        style: 'mapbox://styles/mapbox/streets-v8', // style URL
        center: [-105.634722, 38.985833], // starting position [lng, lat]
        zoom: 7 // starting zoom
      });
      const marker = new mapboxgl.Marker({
        color: "red",
        rotation: 0
      })
        .setLngLat([-106.475700, 39.187500])
        .addTo(map);
      const nav = new mapboxgl.NavigationControl({
        visualizePitch: true,
        showZoom: true,
        showCompass: true
      });
      map.addControl(nav, 'bottom-right');

    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <button v-on:click="makeMap()">Make Map</button> -->
    <div id='map' style='width: 1000px; height: 800px;'></div>

  </div>
</template>

<style>
</style>