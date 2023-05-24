<script setup>
import data from "../../data.json";
import { ref, watch, computed } from "vue";
import Random from "./Random.vue";

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

const selectedName = ref("");

const victory = ref(false);
const clicked = ref(false);

const handleClick = (newId) => {
  selectedName.value = newId;
  clicked.value = true;
};

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

    <div>
      <Random
        @selection="handleClick"
        :collaborators="collaborators"
        :rand-collab="randCollab"
        :rand-ids="randIds"
        :selected-name="selectedName"
      />
    </div>

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
