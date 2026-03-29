<script setup>
defineProps({
  cells: {
    type: Array,
    required: true
    // 9 cells in order: top-left to bottom-right
    // Each: { label: string, sublabel?: string, color: string (text), bgColor: string }
  },
  xAxisLabels: {
    type: Object,
    default: () => ({ left: '', center: '', right: '' })
  },
  yAxisLabels: {
    type: Object,
    default: () => ({ top: '', middle: '', bottom: '' })
  },
  width: { type: Number, default: 550 },
  cellHeight: { type: Number, default: 80 }
})
</script>

<template>
  <div class="matrix-3x3" :style="{ maxWidth: `${width}px` }">
    <div class="grid" :style="{ gridTemplateRows: `repeat(3, ${cellHeight}px)` }">
      <div
        v-for="(cell, idx) in cells"
        :key="idx"
        class="cell"
        :style="{ backgroundColor: cell.bgColor }"
      >
        <span class="cell-label" :style="{ color: cell.color }">{{ cell.label }}</span>
        <span v-if="cell.sublabel" class="cell-sublabel" :style="{ color: cell.sublabelColor || 'rgba(255,255,255,0.7)' }">{{ cell.sublabel }}</span>
      </div>
    </div>
    <div class="x-axis">
      <span>{{ xAxisLabels.left }}</span>
      <span>{{ xAxisLabels.center }}</span>
      <span>{{ xAxisLabels.right }}</span>
    </div>
  </div>
</template>

<style scoped>
.matrix-3x3 {
  margin: 0.5rem auto 0;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2px;
  background: #e5e7eb;
  border-radius: 8px;
  overflow: hidden;
}

.cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 0.5rem;
}

.cell-label {
  font-size: 0.65rem;
  font-weight: 600;
}

.cell-sublabel {
  font-size: 0.55rem;
}

.x-axis {
  display: flex;
  justify-content: space-between;
  margin-top: 0.3rem;
  padding: 0 0.2rem;
}

.x-axis span {
  font-size: 0.65rem;
  color: #888;
}
</style>
