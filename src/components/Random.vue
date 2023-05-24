<script setup lang="ts">
import { toRef, toRefs } from "vue";

const props = defineProps({
  collaborators: {
    type: Array<Object>,
    default: null,
  },
  randCollab: {
    type: Object,
    default: null,
  },
  randIds: {
    type: Array<number>,
    default: [0, 1, 2, 3],
  },
  selectedName: {
    type: String,
    default: "",
  },
});
defineEmits(["selection"]);

const { collaborators } = toRefs(props);
const { randCollab } = toRefs(props);
const { randIds } = toRefs(props);
const selectedName = toRef(props, "selectedName");
</script>

<template>
  <div>
    <div>
      <img :src="randCollab.image" alt="" />
    </div>
    <div
      v-for="(nameId, i) in randIds"
      :key="i"
      @click="$emit('selection', nameId)"
    >
      <input
        type="radio"
        :id="i.toString()"
        :value="nameId"
        v-model="selectedName"
      />
      <label :for="i.toString()">{{ collaborators[nameId].name.first }}</label>
    </div>
  </div>
</template>

<style></style>
