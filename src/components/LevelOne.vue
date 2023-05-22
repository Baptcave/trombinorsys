<script lang="ts">
import data from "../../data.json";
export default {
  setup() {
    const collaborateurs = data;
    return {
      collaborateurs,
    };
  },
};
</script>

<template>
  <h2>Bienvenue dans le niveau 1</h2>
  <div v-for="collab in collaborateurs" :key="collab.id">
    <p>{{ collab.name.first }} {{ collab.name.last }}</p>
    <img :src="collab.image" />
  </div>
</template>

<style>
img {
  width: 150px;
  height: 150px;
}
</style>
