<template>
  <div id="form">
    <div v-if="seen">
      <h1>Formulario Pelicula</h1>
      <div>
        <label>Titulo:</label>
        <input type="text" id="titulo" name="titulo" v-bind:value="movie.Titulo"/>   
      </div>
      <div>
        <label>Director:</label>
        <input type="text" id="director" name="director" v-bind:value="movie.Director"/>   
      </div>
      <div>
        <label>Género:</label>
        <input type="text" id="genero" name="genero"v-bind:value="movie.Genero" />   
      </div>
      <div>
      <label>País:</label>
        <input type="text" id="pais" name="pais"v-bind:value="movie.Pais" />   
      </div>
      <input type="button" id="submit" value="Enviar" v-on:click="enviar"/>
      <input type="button" id="submit" value="Eliminar" v-on:click="eliminar" v-if="movie.Id !== -1"/>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import {EventBus} from './EventBus.js';
  export default {
    name: 'app',
    data () {
      return {
       movie: {},
       seen: true
     }
   },
   methods: {
    enviar: function(){
      let data = {
        Titulo: document.getElementById("titulo").value,
        Director: document.getElementById("director").value,
        Genero: document.getElementById("genero").value,
        Pais: document.getElementById("pais").value,
        Id: this.movie.Id
      }

      if(data.Id  == -1){
        axios.post('http://10.0.2.2:62270/api/Pelicula/' ,data)
        .then(response => {
          this.movie.Id = response.data.Id;
          EventBus.$emit("updatePelicula", this.movie);
        })
      }
      else{
        axios.put('http://10.0.2.2:62270/api/Pelicula/' + data.Id, data)
        .then(response => {
          EventBus.$emit("updatePelicula", this.movie);
        })
      }
    },

    eliminar: function(){
      this.seen = false;
      if(this.movie.Id != -1){
        axios.delete('http://10.0.2.2:62270/api/Pelicula/' + this.movie.Id)
        .then(response=> {
          EventBus.$emit("updatePelicula", this.movie);
        })
      }
    }
  },
  created() {
    this.movie = this.$parent.movie;
  }
}
</script>

<style>
</style>