<template>
<div class="bg-dark text-white">
  <div :key="personaje.id" v-for="personaje in personajes">
    <img v-bind:src="personaje.image" /> 
    <p> <b>{{ personaje.name }}</b></p>
    <p> Origen: {{ personaje.origin.name }}</p>
    <p> Estado: {{ personaje.status }}</p>
    <p> Especie: {{ personaje.specie }}</p>
  </div>
  <button @click="onClick">Enviar</button>
  <form v-on:submit.prevent="onSubmit">
    <input type="number" v-model="form.id" />
    <input type="submit" value="Enviar" />
  </form>
  <ul class="pagination" id="paginator">
    <li class="page-item"><a class="page-link" href="#" @click="test">Previous</a></li>
    <li class="page-item"><a class="page-link" href="#" @click="test">Next</a></li>
  </ul>
</div>

</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      personajes: [],
      form: {
        id: null,
      },
    };
  },
  props: {
    msg: String,
  },
  methods: {
    test(){
      console.log('click')
    },
    async loadCharacters() {
      const response = await axios.get(
        "https://rickandmortyapi.com/api/character"
      );
      const { results } = response.data;
      console.log(results);
      this.personajes = results;
    },
    onClick() {
      this.personajes = [];
    },
    onSubmit() {
      if (this.id <= 0) {
        alert("EL ID TIENE QUE SER MAYOR A 0!");
        return;
      }
    },
  },
  created() {
    this.loadCharacters();
  },
};
</script>
