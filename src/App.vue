<script setup lang="ts">
import { ref } from "vue";
import { PokemonClient } from "pokenode-ts";

let sprite = ref();
let name = ref();
let id = ref();
let height = ref();
let base_experience = ref();
let abilities: Array<String> = [];
let moves: Array<String> = [];
let forms: Array<String> = [];
let types: Array<String> = [];
let held_items: Array<String> = [];

function searchPokemon() {
  const textInput = document.getElementById("search") as HTMLInputElement;

  const api = new PokemonClient();
  api
    .getPokemonByName(textInput.value.toLowerCase())
    .then((data) => {
      sprite.value = data.sprites.front_default;
      name.value = data.name;
      id.value = data.id;
      height.value = data.height;
      base_experience.value = data.base_experience;

      types = [];
      held_items = [];
      moves = [];
      abilities = [];
      forms = [];

      for (let i = 0; i < data.types.length; i++) {
        types.push(data.types[i].type.name);
      }
      for (let i = 0; i < data.held_items.length; i++) {
        held_items.push(data.held_items[i].item.name);
      }
      for (let i = 0; i < data.moves.length; i++) {
        moves.push(data.moves[i].move.name);
      }
      for (let i = 0; i < data.abilities.length; i++) {
        abilities.push(data.abilities[i].ability.name);
      }
      for (let i = 0; i < data.forms.length; i++) {
        forms.push(data.forms[i].name);
      }
    })
    .catch((error) => console.error(error));
}
</script>

<template>
  <h1 id="title">Poké Search</h1>

  <div class="search-container">
    <input type="text" id="search" placeholder="type a pokemon name" />
    <button @click="searchPokemon()">search</button>
  </div>

  <div class="results">
    <img :src="sprite" alt="" />

    <p class="colored">name:</p>
    <p>{{ name }}</p>

    <p class="colored">id:</p>
    <p>{{ id }}</p>

    <p class="colored">height:</p>
    <p>{{ height }}</p>

    <p class="colored">base experience:</p>
    <p>{{ base_experience }}</p>

    <p class="colored">types:</p>
    <ol>
      <li v-for="type in types">
        {{ type }}
      </li>
    </ol>

    <p class="colored">held items:</p>
    <ol>
      <li v-for="item in held_items">
        {{ item }}
      </li>
    </ol>

    <p class="colored">moves:</p>
    <ol>
      <li v-for="move in moves">
        {{ move }}
      </li>
    </ol>

    <p class="colored">abilities:</p>
    <ol>
      <li v-for="ability in abilities">
        {{ ability }}
      </li>
    </ol>

    <p class="colored">forms:</p>
    <ol>
      <li v-for="form in forms">
        {{ form }}
      </li>
    </ol>
  </div>

  <p class="go-to-github-profile">
    Check out my <a href="https://github.com/KonyD">Github</a> profile to see
    more projects like this!
  </p>
</template>

<style scoped>
.go-to-github-profile {
  color: rgb(167, 167, 167);
}
</style>
