<template>
  <v-card>
      <canvas class="kcanvas" id="kanji-canvas" :width="canvasWidth" :height="canvasHeight"
        @mouseleave="stopDrawing" color="white">
        Your browser does not support the canvas element.
      </canvas>
      <div v-if="candidates.length" class="candidates">
        <h3>Candidates:</h3>
        <ul>
          <li class="text-h4" v-for="candidate in candidates" :key="candidate">{{ candidate }}</li>
        </ul>
      </div>
  </v-card>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// Props
const props = defineProps({
  canvasWidth: {
    type: Number,
    default: 256
  },
  canvasHeight: {
    type: Number,
    default: 256
  }
});

// Refs & Variables
const candidates = ref([]);

// Functions
const stopDrawing = () => {
  // Recognize the kanji
  const result = KanjiCanvas.recognize("kanji-canvas");
  candidates.value = result.split('\n').filter(Boolean);
  drawCrossLines(); // Redraw the cross lines after recognizing the kanji
};


// Function to draw the cross lines
const drawCrossLines = () => {
  const canvas = document.getElementById('kanji-canvas');
  const ctx = canvas.getContext('2d');
  ctx.strokeStyle = '#000'; // Color of the lines
  ctx.lineWidth = 0.5; // Thickness of the lines

  // Draw vertical line
  ctx.beginPath();
  ctx.moveTo(canvas.width / 2, 0);
  ctx.lineTo(canvas.width / 2, canvas.height);
  ctx.stroke();

  // Draw horizontal line
  ctx.beginPath();
  ctx.moveTo(0, canvas.height / 2);
  ctx.lineTo(canvas.width, canvas.height / 2);
  ctx.stroke();
};

// Lifecycle Hook
onMounted(() => {
  // Initialize Kanji Canvas
  KanjiCanvas.init("kanji-canvas");
  drawCrossLines();
});
</script>

<style scoped>
.kcanvas {
  border: 1px solid #000;
  background-color: white;
}

.candidates {
  margin-top: 10px;
  text-align: left;
}
</style>