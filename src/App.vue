<template>
  <div id="map"></div>
</template>

<script>
import L from "leaflet";
import "leaflet/dist/leaflet.css";
var center = [-3.10719, -60.0261]; //centro do Mapa
// var map = L.map('map').setView(center, 6);
var editableLayers = new L.FeatureGroup();
var drawPluginOptions = {
  position: "topright",
  draw: {
    polygon: {
      allowIntersection: false, // Restricts shapes to simple polygons
      drawError: {
        color: "#e1e100", // Color the shape will turn when intersects
        message: "<strong>Oh snap!<strong> you can't draw that!", // Message that will show when intersect
      },
      shapeOptions: {
        color: "#97009c",
      },
    },
    // disable toolbar item by setting it to false
    polyline: false,
    circle: false, // Turns off this drawing tool
    rectangle: false,
    marker: false,
  },
  edit: {
    featureGroup: editableLayers, //REQUIRED!!
    remove: false,
  },
};
var drawControl = new L.Control.Draw(drawPluginOptions);
export default {
  name: "App",
  data() {
    return {
      map: undefined,
    };
  },
  methods: {
    initMap() {
      this.map = L.map("map", { drawControl: true}).setView(center, 0); //Criando Mapa
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: 'Data © <a href="http://osm.org/copyright">OpenStreetMap</a>',
        maxZoom: 18,
      }).addTo(this.map);
      L.marker(center).addTo(this.map);
      this.map.addLayer(editableLayers);
      this.map.addControl(drawControl);
      this.map.on("draw:created", function (e) {
        var type = e.layerType,
          layer = e.layer;
        if (type === "marker") {
          layer.bindPopup("POPUP");
        }
        editableLayers.addLayer(layer);
      });
    },
  },
  mounted() {
    this.initMap();
  },
};
</script>

<style>
html,
body {
  height: 100%;
}
#map {
  height: 100%;
}
</style>
