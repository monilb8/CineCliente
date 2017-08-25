<template>
  <div id="home">
    <div id="pelicula" class="menuPrincipal" v-on:click="openComponent">Pelicula</div>
    <div id="entrada" class="menuPrincipal" v-on:click="openComponent">Entrada</div>

    <div id="maestro"></div>
  </div>
</template>

<script>
  import axios from "axios"
  import Vue from 'vue'
  import MaestroPelicula from "./AppPelicula.vue"
  import MaestroEntrada from "./AppEntrada.vue"
  export default {
    name: 'app',
    data () {
      return {

      }
    },
    methods: {
      openComponent: function(e){
        switch(e.target.id){
          case "pelicula": 
          new Vue({
            el: '#maestro',
            render: h => h(MaestroPelicula)
          })

          break;
          case "entrada": 
          new Vue({
            el: '#maestro',
            render: h => h(MaestroEntrada)
          })

          break;
        }
      }

    },

  }
</script>

<style>

</style>