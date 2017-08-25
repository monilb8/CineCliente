<template>
  <div id="form">
    <div v-if="seen">
      <h1>Formulario Entrada</h1>
      <div>
        <label>Pelicula:</label>
        <input type="text" id="peli" name="peli" v-bind:value="ticket.Pelicula"/>   
      </div>
      <div>
        <label>Duracion:</label>
        <input type="number" id="duracion" name="duracion" v-bind:value="ticket.Duracion"/>   
      </div>
      <div>
        <label>Butaca:</label>
        <input type="int" id="butaca" name="butaca"v-bind:value="ticket.Butaca" />   
      </div>
      <input type="button" id="submit" value="Enviar" v-on:click="enviar"/>
      <input type="button" id="submit" value="Eliminar" v-on:click="eliminar" v-if="ticket.Id !== -1"/>
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
       ticket: {},
       seen: true
     }
   },
   methods: {
    enviar: function(){
      let data = {
        Pelicula: document.getElementById("peli").value,
        Duracion: document.getElementById("duracion").value,
        Butaca: document.getElementById("butaca").value,
        Id: this.ticket.Id
      }

      if(data.Id  == -1){
        axios.post('http://10.0.2.2:62270/api/Entradas/' ,data)
        .then(response => {
          this.ticket.Id = response.data.Id;
          EventBus.$emit("updateEntrada", this.ticket);
        })
      }
      else{
        axios.put('http://10.0.2.2:62270/api/Entradas/' + data.Id, data)
        .then(response => {
          EventBus.$emit("updateEntrada", this.ticket);
        })
      }
    },

    eliminar: function(){
      this.seen = false;
      if(this.ticket.Id != -1){
        axios.delete('http://10.0.2.2:62270/api/Entradas/' + this.ticket.Id)
        .then(response=> {
          EventBus.$emit("updateEntrada", this.ticket);
        })
      }
    }
  },
  created() {
    this.ticket = this.$parent.ticket;
  }
}
</script>

<style>
</style>