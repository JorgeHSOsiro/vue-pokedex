<template>
  <RouterLink :to="{name: 'details', params: {id: pokemonName}}">
    <div class="pokeCard" >
      <img :src="sourceImage" alt="imagem do pokemon">
      <div class="pokeName">
        <p>{{ pokemonName }}</p>
      </div>
    </div>
  </RouterLink>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent ({
  name: "PokemonCard",
  props: {
    sourceImage: {
      type: String
    },
    pokemonName: {
      type: String
    },
  }
}) 
</script>
  
<style lang="scss">
  a {
    text-decoration: none;
  }
  .pokeCard {
    display: flex;
    flex-direction: column;
    border: 2px solid #41FAE7;
    border-radius: 10px;
    margin-top: 10px;
    width: 250px;
    cursor: pointer;
    
    .pokeName {
      background-color: #41FAE7;
      p {
        color: white;
        font-size: 1.5em;
      }
    }
  }

  .pokeCard:hover {
    opacity: 50%;
  }
</style>