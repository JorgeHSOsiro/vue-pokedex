<template>
  <div class="form" role="form" aria-label="Barra de busca de pokémon">
    <input type="text" placeholder=" ex.: pikachu" v-model="pokemon"/>
    <button @click="search">
      <span class="icon">
        <i class="fas fa-search"></i>
      </span>
    </button>
  </div>
 
</template>

<script lang="ts">
import { defineComponent} from "vue";

export default defineComponent({
  name: "SearchBar",
  emits: ["search"],
  data () {
    return {
      pokemon: '',
    }
  },
  methods: {
    search () {
      this.$emit('search', this.pokemon)
    }
  }
  
});
</script>

<style lang="scss">
.form {
  display: flex;
  align-items: center;
  justify-content: center;
  input {
    padding: 6px;
    font-size: 17px;
  }
  input:focus {
    outline: none;
  }
  button {
    padding: 6px 10px;
    font-size: 21px;
    border: none;
    background-color: #3EA6F0;
    color: white;
    cursor: pointer;
  }
  button:hover {
    opacity: 0.8;
  }
}
</style>