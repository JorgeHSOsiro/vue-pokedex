<template>
  <div class="detailsView" >
    <div class="pokeTable">
      <div class="pokeBox">
        <img :src="pokemon?.sprites?.front_default" alt="imagem do pokemon" />
      </div>
      <table class="characteristics">
        <tr>
          <th v-for="(stat, index) in pokemon?.stats" :key="index">
            {{ stat.stat.name }}
          </th>
        </tr>
        <tr>
          <td v-for="(stat, index) in pokemon?.stats" :key="index">
            {{ stat.base_stat }}
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import api from '@/services/api';
import { defineComponent, onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

export default defineComponent({
  name: 'DetailsView',
  setup() {
    const route = useRoute();
    const pokemon = ref();

    const fetchPokemon = () => api.get(`/pokemon/${route.params.id}`).then(res => pokemon.value = res.data).catch(e => console.log(e))

    onMounted(fetchPokemon);

    return { pokemon }
  }
}) 
</script>

<style lang="scss">
  .detailsView {
    display: flex;
    justify-content: center;
    
    .pokeTable {
      display: flex;
      justify-content: center;
      background-color: #FAE05C;
      border-radius: 8px;
      @media screen and (max-width: 600px) {
        align-items: center;
        flex-direction: column;
      }
      
      .pokeBox{
        background-color: #FFF;
        margin: 2px 0 2px 2px;
        border-radius: 8px;
        width: 30%;
        text-align: center;
        display: block;
        width: 50%;
        img {
          width: 70%;
        }
      }
      .characteristics {
        tr {
          th {
            width: 90px;
            height: 60%;
            background-color: #41FAE7;
            color: #FFF;
            border-radius: 8px 8px 0 0;
          }
          td {
            background-color: #FFF;
            border-radius: 0 0 8px 8px ;
          }
        }
    }
    
  }
  

  }
</style>