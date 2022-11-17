<template>
  <div id="app">
    <BarraNavegacion @tipoMapa="cambiarTipoMapa" />
    <DibujoMapa :tipoMapa="tipoMapa"/>
  </div>
</template>

<script>
import BarraNavegacion from "./components/BarraNavegacion.vue"
import DibujoMapa from "./components/DibujoMapa.vue"

export default {
  name: 'App',
  components: {
    DibujoMapa, BarraNavegacion
  },
  methods: {
    cambiarTipoMapa: function(url){
      this.tipoMapa = url;
    }
  },
  data: function(){
    return{
      tipoMapa: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'
    }
  }
}
</script>

<style>
  @import "./assets/css/App.css"
</style>
