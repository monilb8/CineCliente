<template>
  <div id="maestro">
    <table v-if="movies && movies.length">
      <tr>
        <th>Titulo</th>
        <th>Director</th>
        <th>Genero</th>
        <th>Pais</th>
      </tr>
      <tr v-for="movie of movies" v-on:click="detail" v-bind:id="movie.Id">
        <td>{{movie.Titulo}}</td>
        <td> {{movie.Director}}</td>
        <td> {{movie.Genero}}</td>
        <td> {{movie.Pais}}</td>
      </tr>
    </table>
    <input type="button" id="submit" value="Nuevo" v-on:click="nuevo"/>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import Form from './FormPelicula.vue'
  import {EventBus} from './EventBus.js';
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        movies: undefined
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        if(id == ""){
          id = e.target.parentNode.id;
        }
        this.movies.forEach((p, index) => {
          if(p.Id == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                movie: p
              },
            });
          }
        });
      },
      nuevo: function (e) {
        new Vue({
          el: '#form',
          render: h => h(Form),
          data: {
            movie: {
              Titulo: undefined,
              Director: undefined,
              Genero: undefined,
              Pais: undefined,
              Id:-1
            }
          },
        });

      },
      init: function(){
        axios.get('http://10.0.2.2:62270/api/Pelicula/')
        .then(response => {
          this.movies = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('updatePelicula', (() => {
        this.init();
      }));

    }

  }
</script>

<style>
</style>
