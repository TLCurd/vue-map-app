
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
        var description = "";
        this.peaks.forEach(peak => {
          console.log(this.peaks.count);
          description = `<strong>${peak.name}</strong><p> <a href="http://localhost:8080/peaks/${peak.id}"><img src=${peak.photo} target="_blank" title="Learn more about ${peak.name}"></a> <p>Located in the ${peak.range} mountain range</p>
          <p>Elevation:${peak.elevation}</p>
          <p>Official 14er: ${peak.official_14er}</p>
          <p>The summit is ${peak.prominence} above the nearest peak.</p></p>`;
          const marker = new mapboxgl.Marker({
            color: "red",
            rotation: 0,
          })
            .setLngLat([peak.long, peak.lat])
            .setPopup(new mapboxgl.Popup({ offset: 25 }) //add popups
              .setHTML(
                description
              )
            )
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
img {
  width: 200px
}

.mapboxgl-popup {
  max-width: 400px;
}

.mapboxgl-popup-content {
  text-align: center;
  font-family: 'Open Sans', sans-serif;
}

marker {
  background-image: url('https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer
}
</style>