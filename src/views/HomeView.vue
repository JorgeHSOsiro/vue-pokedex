<template>
  <div class="home">
    <SearchBar @search="searchPokemon" />
    <div v-if="!nullPokemon" class="pokeList">
      <PokemonCard 
        v-for="(pokemon, index) in pokemonList" 
        :key="index" 
        :source-image="pokemon?.sprites?.front_default" 
        :pokemon-name="pokemon?.name" 
      />
    </div>
    <div v-if="nullPokemon">
      <p v-if="notFound">Busque um pokemon</p>
      <p v-if="!notFound">{{ message }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SearchBar from '@/components/SearchBar.vue';
import PokemonCard from '@/components/PokemonCard.vue'
import api from '@/services/api';
import { IPokemon } from '@/interfaces/IPokemon';

export default defineComponent({
  name: 'HomeView',
  components: {
    SearchBar,
    PokemonCard,
  },
  data () {
    return {
      message: '',
      pokemonList: [] as IPokemon[],
      pokemon: Object as unknown as IPokemon,
      secondEvolution: Object as unknown as IPokemon,
      thirdEvolution: Object as unknown as IPokemon,
    }
  },
  methods: {
    searchPokemon(name: string) {
      this.pokemonList = [];

      api.get(`/pokemon/${name}`)
        .then(res => this.pokemon = res.data)
        .then(() => this.pokemon !== undefined && this.pokemonList.push(this.pokemon))
        .then(() => api.get(`pokemon-species/${name}`))
        .then((res) => api.get(res.data.evolution_chain.url))
        .then(res => api.get(res.data.chain.evolves_to[0]?.species.url))
        .then(res => api.get(`/pokemon/${res.data.name}`))
        .then(res => this.secondEvolution = res.data)
        .then(() => this.secondEvolution.name !== this.pokemon.name && this.pokemonList.push(this.secondEvolution))
        .then(() => api.get(`pokemon-species/${name}`))
        .then((res) => api.get(res.data.evolution_chain.url))
        .then(res => api.get(res.data.chain.evolves_to[0]?.evolves_to[0].species.url))
        .then(res => api.get(`/pokemon/${res.data.name}`))
        .then(res => this.thirdEvolution = res.data)
        .then(() => this.thirdEvolution.name !== this.pokemon.name && this.pokemonList.push(this.thirdEvolution))
        .catch(e =>{
          this.message = 'Pokemon not found! Check if name is correct.'
          console.log(e)
        })
    },
  },
  computed: {
    nullPokemon(): boolean {
      return this.pokemonList?.length === 0 
    },
    notFound(): boolean {
      return this.message === ''
    }
  }
}
);
</script>

<style lang="scss">
 .pokeList {
  display: flex;
  justify-content: center;
  gap: 1rem;
  @media screen and (max-width: 600px) {
    align-items: center;
    flex-direction: column;
  }
 }
</style>