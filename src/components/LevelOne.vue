<script setup>
import data from "../../data.json";
import { ref, computed, watch } from "vue";

const collaborators = data;
const max = collaborators.length;

let rand = ref(Math.floor(Math.random() * max));
const randCollab = computed(() => collaborators[rand.value]);

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

  return _.shuffle(selected);
});

const reload = () => {
  selectedName.value = "";
  victory.value = false;
  clicked.value = false;
  const oldRand = rand.value;
  while (rand.value === oldRand) {
    rand.value = Math.floor(Math.random() * max);
  }
};

const handleClick = () => {
  clicked.value = true;
};

const selectedName = ref("");

const victory = ref(false);
const clicked = ref(false);

watch(selectedName, () => {
  const selectedId = parseInt(selectedName.value, 10) + 1;
  if (selectedId.toString() === randCollab.value.id.toString()) {
    victory.value = true;
  } else {
    victory.value = false;
  }
});
</script>

<template>
  <div>
    <h2>Bienvenue dans le niveau 1</h2>
    <button @click="reload">Recharger</button>

    <!--COMPOSANT AVEC EMIT/PROPS -->
    <div>
      <img :src="randCollab.image" alt="" />
    </div>
    <div v-for="(nameId, i) in randIds" :key="i" @click="handleClick">
      <input
        type="radio"
        :id="i.toString()"
        :value="nameId"
        v-model="selectedName"
      />
      <label :for="i.toString()">{{ collaborators[nameId].name.first }}</label>
    </div>
    <!-- END COMPOSANT -->

    <div class="" v-if="clicked">
      <p class="victory" v-if="victory">Gagné !</p>
      <p class="failure" v-else>Essaye encore...</p>
    </div>
  </div>
</template>

<style lang="scss">
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
