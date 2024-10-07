<template>
  <v-card class="d-flex flex-column align-center py-5">
    <canvas class="kcanvas" :id="`kanji-canvas-${canvasId}`" :width="canvasWidth" :height="canvasHeight"
      @mouseup="stopDrawing" @touchend="stopDrawing" @touchstart="preventScroll" @touchmove="preventScroll"
      color="white">
      Your browser does not support the canvas element.
    </canvas>
    <v-card-actions class="d-flex justify-center">
      <v-btn icon="mdi-undo" @click="undoStroke"></v-btn>
      <v-btn icon="mdi-trash-can-outline" @click="clearCanvas"></v-btn>
    </v-card-actions>
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
  },
  canvasId: {
    type: Number,
    required: true
  }
});

// Emit
const emit = defineEmits(['update:candidates']);

// Refs & Variables
const candidates = ref([]);

// Functions
const stopDrawing = async () => {
  // Recognize the kanji
  setTimeout(() => {
    const result = KanjiCanvas.recognize(`kanji-canvas-${props.canvasId}`);
    candidates.value = result.split('\n').filter(Boolean);
    emit('update:candidates', candidates.value); // Emit the recognized characters
    drawCrossLines();
  }, 100);
};

const clearCanvas = () => {
  KanjiCanvas.erase(`kanji-canvas-${props.canvasId}`);
  candidates.value = [];
  drawCrossLines();
};


const undoStroke = () => {
  KanjiCanvas.deleteLast(`kanji-canvas-${props.canvasId}`);
  drawCrossLines();
};
const preventScroll = (event) => {
  event.preventDefault();
};

// Function to draw the cross lines
const drawCrossLines = () => {
  const canvas = document.getElementById(`kanji-canvas-${props.canvasId}`);
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
  KanjiCanvas.init(`kanji-canvas-${props.canvasId}`);
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