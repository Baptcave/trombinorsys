<script lang="ts">
import data from "../../data.json";
import { ref, computed, watch } from "vue";
export default {
  setup() {
    const collaborateurs = data;
    const max = collaborateurs.length;
    let rand = ref(Math.floor(Math.random() * max));
    const randCollab = computed(() => collaborateurs[rand.value]);
    // console.log("id photo affichée :", randCollab.value.id);

    const randIds = computed(() => {
      let selected = [];
      selected.push(randCollab.value.id - 1);
      for (let i = 0; i < 3; i++) {
        let oneRandomIndex = Math.floor(Math.random() * max);
        while (selected.includes(oneRandomIndex)) {
          oneRandomIndex = Math.floor(Math.random() * max);
        }
        selected = [...selected, oneRandomIndex];
      }
      // console.log(selected);

      return selected;
    });

    const reload = () => {
      const oldRand = rand.value;
      while (rand.value === oldRand) {
        rand.value = Math.floor(Math.random() * max);
      }
    };

    const selectedName = ref("");

    const victory = ref(false);
    const failure = ref(false);

    watch(selectedName, () => {
      console.log("sélectionné", selectedName);
      console.log("photo affichée", randCollab.value.id);

      const selectedId = parseInt(selectedName.value, 10) + 1;
      if (selectedId.toString() === randCollab.value.id.toString()) {
        victory.value = true;
        failure.value = false;
      } else {
        failure.value = true;
        victory.value = false;
      }
    });

    return {
      collaborateurs,
      max,
      randCollab,
      reload,
      randIds,
      selectedName,
      victory,
      failure,
    };
  },
};
</script>

<template>
  <h2>Bienvenue dans le niveau 1</h2>
  <button @click="reload">Recharger</button>
  <div>
    <img :src="randCollab.image" />
  </div>
  <div v-for="(nameId, i) in randIds" :key="i">
    <input type="radio" id="nameId" :value="nameId" v-model="selectedName" />
    <label for="nameId">{{ collaborateurs[nameId].name.first }}</label>
  </div>
  <p class="victory" v-if="victory">Gagné !</p>
  <p class="failure" v-else-if="failure">Essaye encore...</p>
</template>

<style>
img {
  width: 150px;
  height: 150px;
}

.victory {
  font-weight: bold;
  color: green;
}

.failure {
  font-weight: bold;
  color: red;
}
</style>
