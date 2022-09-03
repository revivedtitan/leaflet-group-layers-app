<template>
  <div id="map"></div>
</template>

<script>
// Import leaflet mapping library
import "leaflet/dist/leaflet.css";
import L from "leaflet";

// Import group layer control library
import 'leaflet-groupedlayercontrol';
import 'leaflet-groupedlayercontrol/dist/leaflet.groupedlayercontrol.min.css';

// This is a quirk where the pathway to the default marker icon is broken
// If you're using custom icons or an icon library you wont need this.
delete L.Icon.Default.prototype._getIconUrl;

L.Icon.Default.mergeOptions({
   iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
   iconUrl: require('leaflet/dist/images/marker-icon.png'),
   shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

export default {
  name: 'App',
  data() {
   return{
     center: [37,7749, -122,4194]
   }},
  methods: {
   initialize: function () {

    var basemaps = {
      Watercolor: L.tileLayer('https://stamen-tiles-{s}.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.{ext}', {
        subdomains: 'abcd',
        minZoom: 1,
        maxZoom: 16,
        ext: 'jpg'
      }),
      Streets: L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
      })
    };

  var groups = {
    cities: new L.LayerGroup(),
    restaurants: new L.LayerGroup(),
    dogs: new L.LayerGroup(),
    cats: new L.LayerGroup()
  };

  L.marker([39.61, -105.02]).bindPopup('Littleton, CO.').addTo(groups.cities);
  L.marker([39.74, -104.99]).bindPopup('Denver, CO.').addTo(groups.cities);
  L.marker([39.73, -104.8]).bindPopup('Aurora, CO.').addTo(groups.cities);
  L.marker([39.77, -105.23]).bindPopup('Golden, CO.').addTo(groups.cities);

  L.marker([39.69, -104.85]).bindPopup('A restaurant').addTo(groups.restaurants);
  L.marker([39.69, -105.12]).bindPopup('A restaurant').addTo(groups.restaurants);

  L.marker([39.79, -104.95]).bindPopup('A dog').addTo(groups.dogs);
  L.marker([39.79, -105.22]).bindPopup('A dog').addTo(groups.dogs);

  L.marker([39.59, -104.75]).bindPopup('A cat').addTo(groups.cats);
  L.marker([39.59, -105.02]).bindPopup('A cat').addTo(groups.cats);

    var map = L.map('map', {
      center: [39.73, -104.99],
      zoom: 10,
      layers: [basemaps.Watercolor, groups.cities]
    });

     // Overlay layers are grouped
     var groupedOverlays = {
      "Landmarks": {
        "Cities": groups.cities,
        "Restaurants": groups.restaurants
      },
      "Random": {
        "Dogs": groups.dogs,
        "Cats": groups.cats
      }
    };

    // Use the custom grouped layer control, not "L.control.layers"
    L.control.groupedLayers(basemaps, groupedOverlays).addTo(map);

   },
 },
 mounted() {
   this.initialize();
 },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#map {
  height: 95vh;
}
</style>
