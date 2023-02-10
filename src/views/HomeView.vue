<template>
  <div class="home">
    <SearchBar @search="searchPokemon" />
    <div>
      <img :src="pokemon?.sprites?.front_default" alt="imagem do pokemon">
      <p>{{ pokemon?.name }}</p>
    </div>
    <div>
      <img :src="secondEvolution?.sprites?.front_default" alt="imagem do pokemon" />
      <p>{{ secondEvolution?.name }}</p>
    </div>
    <div>
      <img :src="thirdEvolution?.sprites?.front_default" alt="imagem do pokemon" />
      <p>{{ thirdEvolution?.name }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SearchBar from '@/components/SearchBar.vue';
import api from '@/services/api';
import { IPokemon } from '@/interfaces/IPokemon';

export default defineComponent({
  name: 'HomeView',
  components: {
    SearchBar
  },
  data () {
    return {
      pokemon: Object as unknown as IPokemon | null,
      secondEvolution: Object as unknown as IPokemon | null,
      thirdEvolution: Object as unknown as IPokemon | null,
    }
  },
  methods: {
    searchPokemon(name: string) {
      this.pokemon = null;
      this.secondEvolution = null;
      this.thirdEvolution = null;
      api.get(`/pokemon/${name}`)
        .then(res => this.pokemon = res.data)
        .then(() => api.get(`pokemon-species/${name}`))
        .then((res) => api.get(res.data.evolution_chain.url))
        .then(res => api.get(res.data.chain.evolves_to[0]?.species.url))
        .then(res => api.get(`/pokemon/${res.data.name}`))
        .then(res => this.secondEvolution = res.data)
        .then(() => api.get(`pokemon-species/${name}`))
        .then((res) => api.get(res.data.evolution_chain.url))
        .then(res => api.get(res.data.chain.evolves_to[0]?.evolves_to[0].species.url))
        .then(res => api.get(`/pokemon/${res.data.name}`))
        .then(res => this.thirdEvolution = res.data)
        .catch(e => console.log(e))
    }
  },
}
);
</script>
