<template>
  <div id="maestro">
    <table v-if="tickets && tickets.length">
      <tr>
        <th>Pelicula</th>
        <th>Duracion</th>
        <th>Butaca</th>
      </tr>
      <tr v-for="ticket of tickets" v-on:click="detail" v-bind:id="ticket.Id">
        <td> {{ticket.Pelicula}}</td>
        <td> {{ticket.Duracion}}</td>
        <td> {{ticket.Butaca}}</td>
      </tr>
    </table>
    <input type="button" id="submit" value="Nuevo" v-on:click="nuevo"/>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import Form from './FormEntrada.vue'
  import {EventBus} from './EventBus.js';
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        tickets: undefined
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        if(id == ""){
          id = e.target.parentNode.id;
        }
        this.tickets.forEach((p, index) => {
          if(p.Id == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                ticket: p
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
            ticket: {
              Pelicula: undefined,
              Duracion: undefined,
              Butaca: undefined,
              Id:-1
            }
          },
        });

      },
      init: function(){
        axios.get('http://10.0.2.2:62270/api/Entradas/')
        .then(response => {
          this.tickets = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('updateEntrada', (() => {
        this.init();
      }));

    }

  }
</script>

<style>
</style>
