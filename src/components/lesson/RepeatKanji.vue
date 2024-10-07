<template>
  <v-container>
    <v-row>
      <v-col v-for="i in 5" :key="i" cols="12" md="4">
        <KanjiCanvas :canvasId="i" @update:candidates="handleCandidates(i, $event)" />
        <ul v-if="candidates[i-1].length">
          <li v-for="candidate in candidates[i-1]" :key="candidate">{{ candidate }}</li>
        </ul>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from 'vue';

// Refs & Variables
const candidates = ref(Array.from({ length: 5 }, () => []));

// Functions
const handleCandidates = (canvasId, newCandidates) => {
  // Update the specific canvas's candidates array
  candidates.value[canvasId - 1] = newCandidates.slice(0, 5);
};
</script>