<template>
  <div class="home">
    <SearchBar />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SearchBar from '@/components/SearchBar.vue';

export default defineComponent({
  name: 'HomeView',
  components: {
    SearchBar
},
});
</script>
