<template>
  <div class="BarraNavegacion">
    <span class="nav-bar" id="btnMenu" @click="mostrarMenu"><i class="fas fa-bars"></i> Menú</span>
    <nav class="main-nav">
      <ul class="menu" id="menu">
        <li class="titulo-barra">
          <i class="fas fa-globe-americas"></i> Geolocalización
        </li>
        
        <li class="contenedor-buscador-fecha">
          <i class="fas fa-calendar-alt"></i> Fecha&nbsp;&nbsp;&nbsp;
          <input type="text" class="miInput" id="buscador-fecha" autocomplete="off" readonly>
        </li>

        <li class="menu__item container-submenu">
          <a href="#" class="menu__link submenu-btn" id="histogramas" @click="desplegarSubmenu">
            <i class="fas fa-chart-bar"></i> Histogramas <i class="fas fa-caret-down"></i>
          </a>
          <ul class="submenu">
            <li class="menu__item"><a href="" class="menu__link">Calles</a></li>
            <li class="menu__item"><a href="" class="menu__link">Ciudades</a></li>
          </ul>
        </li>
        
        <li class="menu__item container-submenu configuracion">
          <a href="#" class="menu__link submenu-btn" id="configuracionMapa" @click="desplegarSubmenu">
            <i class="fas fa-cog"></i> Configuración Mapa <i class="fas fa-caret-down"></i>
          </a>
          <ul class="submenu">
            <li class="menu__item container-submenu2">
              <a href="#" class="menu__link submenu-btn2" id="tipoMapa" @click="desplegarSubmenu2">
                <span class="icono-izquierdo"><i class="fas fa-caret-left"></i>&nbsp;&nbsp;</span>
                Tipo de Mapa
                <span class="icono-abajo">&nbsp;<i class="fas fa-caret-down"></i></span>
              </a>
              <ul class="submenu2">
                <li class="menu__item" v-for="mapa in tipoMapa" v-bind:key="mapa.url" :id="mapa.url"
                    @click="cambiarTipoMapa(mapa.url)">
                  <span class="menu__link">{{mapa.nombre}}</span>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import Pikaday from "pikaday"

export default{
  name: 'BarraNavegacion',
  methods: {
    //Metodos enfocado en las funcionalidades de la barra
    cambiarTipoMapa: function(url){
      //Se cambia el background del tipo de mapa elegido
      const tipoMapaElegido = document.getElementById(url);
      const contenedor      = tipoMapaElegido.parentNode;
      const hijos           = contenedor.childNodes;

      hijos.forEach(li => li.style.backgroundColor = "#0f0d0d");
      tipoMapaElegido.style.backgroundColor = "#91380c";

      //Se envia la seleccion al componente principal
      this.$emit("tipoMapa",url);
    },
    //Metodos enfocados en el diseño de la barra
    mostrarMenu: function(){
      const menu = document.querySelector("#menu");
      menu.classList.toggle("mostrar");
    },
    desplegarSubmenu: function(event){
      if(window.innerWidth<1024){
        const subMenuBtn = document.getElementById(event.target.id);
        const subMenu    = subMenuBtn.nextElementSibling;

        if(subMenu.classList.contains("desplegar")){
          subMenu.classList.remove("desplegar");
          subMenu.removeAttribute("style");
        }
        else{
          subMenu.classList.add("desplegar");
          subMenu.style.display= "block";
        }
      }
    },
    desplegarSubmenu2: function(event){
      if(window.innerWidth<1024){
        const subMenuBtn = document.getElementById(event.target.id);
        const subMenu    = subMenuBtn.nextElementSibling;

        if(subMenu.classList.contains("desplegar2")){
          subMenu.classList.remove("desplegar2");
          subMenu.removeAttribute("style");
        }
        else{
          subMenu.classList.add("desplegar2");
          subMenu.style.display= "block";
        }
      }
    }
  },
  mounted: function(){
    //Configuracion del controlador de fecha
    new Pikaday({
        field: document.getElementById('buscador-fecha'),
        format: 'D-M-YYYY',
        toString(date) {
            // you should do formatting based on the passed format,
            // but we will just return 'D/M/YYYY' for simplicity
            const day = date.getDate();
            const month = date.getMonth() + 1;
            const year = date.getFullYear();
            return `${day}-${month}-${year}`;
        },
        parse(dateString) {
            // dateString is the result of `toString` method
            const parts = dateString.split('-');
            const day = parseInt(parts[0], 10);
            const month = parseInt(parts[1], 10) - 1;
            const year = parseInt(parts[2], 10);
            return new Date(year, month, day);
        }
    });

    //Inicializacion de algunos valores al iniciar la aplicacion
    document.getElementById(this.tipoMapa[0].url).style.backgroundColor = "#91380c";
  },
  data: function(){
    return{
      tipoMapa: [
        {
          nombre: 'Calles',
          url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'
        },
        {
          nombre: 'Terreno',
          url: 'https://stamen-tiles.a.ssl.fastly.net/terrain/{z}/{x}/{y}.jpg'
        },
        {
          nombre: 'Blanco y negro',
          url: 'https://stamen-tiles.a.ssl.fastly.net/toner/{z}/{x}/{y}.png'
        },
        {
          nombre: 'Satelital',
          url: 'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}'
        },
        {
          nombre: 'Topográfico',
          url: 'https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png'
        }
      ]
    }
  }
}
</script>

<style scoped>
  @import '../../node_modules/pikaday/css/pikaday.css';
  @import '../assets/css/BarraNavegacion.css';
</style>