<template>
  <div class="bg-dark text-white">
    <div :key="personaje.id" v-for="personaje in personajes">
      <img v-bind:src="personaje.image" />
      <p>
        <b>{{ personaje.name }}</b>
      </p>
      <p>Origen: {{ personaje.origin.name }}</p>
      <p>Estado: {{ personaje.status }}</p>
      <p>Especie: {{ personaje.specie }}</p>
    </div>
    <button @click="onClick">Enviar</button>
    <form v-on:submit.prevent="onSubmit">
      <input type="number" v-model="form.id" />
      <input type="submit" value="Enviar" />
    </form>
    <ul class="pagination" id="paginator">
      <li class="page-item">
        <a class="page-link" href="#" @click="prevNext(-1)">Previous</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#" @click="prevNext(1)">Next</a>
      </li>
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
      totalPages: '',
      currentPage: '',
      nextUrl: '',
      prevUrl: ''
    };
  },
  props: {
    msg: String,
  },
  methods: {
    prevNext(nextPrev){
      //Next Page
      if(nextPrev <= this.totalPages && nextUrl !== null ){
         this.fillPage(this.nextUrl)
      }
      //Previous Page
      if(nextPrev > 0 && prevUrl !== null){
        this.fillPage(this.prevUrl)
      }
    },
    async loadCharacters(url) {
      const response = await this.fillPage(url)
      const { info, results } = response.data;
      this.totalPages = info.pages
      this.currentPage = info.prev === null ? 1 : info.prev + 1
      this.nextUrl = info.next
      this.prevUrl = info.prev
      
      console.log(results);
      this.personajes = results;
    },
    onClick() {
      this.personajes = [];
    },
    async fillPage(url) {
      if(url !== null && url !== undefined){
       return await axios.get(url)
      }
      else{
        return await axios.get("https://rickandmortyapi.com/api/character")
      }
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
