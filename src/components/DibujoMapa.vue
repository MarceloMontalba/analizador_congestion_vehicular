<template>
  <l-map style="width:100%;height:100vh" class="mapa" :zoom="zoom" :center="center" :options="opciones">
    <l-tile-layer :url="tipoMapa"></l-tile-layer>
    <l-marker :lat-lng="markerLatLng"></l-marker>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker } from 'vue2-leaflet';

export default{
  name: 'DibujoMapa',
  props: ['tipoMapa'],
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  data () {
    return {
      zoom: 13,
      center: [-38.7396500, -72.5984200],
      markerLatLng: [51.504, -0.159],
      opciones:{
        attributionControl: false,
        zoomControl: false
      }
    };
  }
}
</script>

<style>
  @import '../assets/css/DibujoMapa.css';
</style>