
<script>
import axios from 'axios'
import mapboxgl from 'mapbox-gl'; // or "const mapboxgl = require('mapbox-gl');"
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      peaks: []
    };
  },
  mounted: function () {
    this.peaksIndex();
    this.makeMap();
  },
  methods: {
    peaksIndex: function () {
      console.log('indexing Peaks...');
      axios.get(`http://localhost:3000/peaks.json`).then(response => {
        console.log(response.data);
        this.peaks = response.data
      })
    },
    makeMap: function () {
      mapboxgl.accessToken = process.env.VUE_APP_MAP_KEY;
      const map = new mapboxgl.Map({
        container: 'map', // container ID
        style: 'mapbox://styles/mapbox/streets-v8', // style URL
        center: [-105.634722, 38.985833], // starting position [lng, lat]
        zoom: 7 // starting zoom
      });
      axios.get(`http://localhost:3000/peaks.json`).then(response => {
        console.log(response.data);
        this.peaks = response.data;
        this.peaks.forEach(peak => {
          console.log(peak.name)
          const marker = new mapboxgl.Marker({
            color: "red",
            rotation: 0
          })
            .setLngLat([peak["long"], peak["lat"]])
            .addTo(map)
        })
      });
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
    <hr>
    <!-- {{ peaks }} -->
    <!-- <button v-on:click="makeMap()">Make Map</button> -->
    <div id='map' style='width: 1000px; height: 800px;'></div>

  </div>
</template>

<style>
</style>