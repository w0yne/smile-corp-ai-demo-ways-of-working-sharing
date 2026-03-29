<script setup>
defineProps({
  cells: {
    type: Array,
    required: true
    // 25 cells (5x5 grid), each: { color: string, label?: string }
  },
  risks: {
    type: Array,
    default: () => []
    // Each: { id: string, description: string }
  },
  maxWidth: { type: Number, default: 500 },
  cellSize: { type: Number, default: 48 }
})

// Color mapping for severity levels
const getRowLabel = (idx) => {
  const labels = ['5-High', '4', '3', '2', '1-Low']
  return labels[idx]
}

const getColLabel = (idx) => {
  const labels = ['1-Low', '2', '3', '4', '5-High']
  return labels[idx]
}
</script>

<template>
  <div class="risk-matrix" :style="{ maxWidth: `${maxWidth}px` }">
    <div class="grid-container">
      <!-- Y-axis labels -->
      <div class="y-labels">
        <div
          v-for="idx in 5"
          :key="`y-${idx}`"
          class="y-label"
          :style="{ height: `${cellSize}px` }"
        >
          {{ getRowLabel(idx - 1) }}
        </div>
      </div>

      <!-- Grid -->
      <div class="grid" :style="{ gridTemplateColumns: `repeat(5, ${cellSize}px)`, gridTemplateRows: `repeat(5, ${cellSize}px)` }">
        <div
          v-for="(cell, idx) in cells"
          :key="idx"
          class="cell"
          :style="{ backgroundColor: cell.color }"
        >
          <span v-if="cell.label" :style="{ color: cell.textColor || 'white' }">{{ cell.label }}</span>
        </div>
      </div>
    </div>

    <!-- X-axis labels -->
    <div class="x-labels" :style="{ paddingLeft: '50px' }">
      <div
        v-for="idx in 5"
        :key="`x-${idx}`"
        class="x-label"
        :style="{ width: `${cellSize}px` }"
      >
        {{ getColLabel(idx - 1) }}
      </div>
    </div>
    <div class="x-axis-title">Impact →</div>

    <!-- Risk legend -->
    <div v-if="risks.length > 0" class="risk-legend">
      <span v-for="(risk, idx) in risks" :key="idx">
        <strong>{{ risk.id }}</strong> {{ risk.description }}
        <template v-if="idx < risks.length - 1">&nbsp;&nbsp;</template>
      </span>
    </div>
  </div>
</template>

<style scoped>
.risk-matrix {
  margin: 0.5rem auto 0;
}

.grid-container {
  display: flex;
  gap: 2px;
}

.y-labels {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding-right: 4px;
}

.y-label {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.55rem;
  font-weight: 600;
  color: #888;
}

.grid {
  display: grid;
  gap: 2px;
}

.cell {
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cell span {
  font-size: 0.6rem;
  font-weight: 700;
}

.x-labels {
  display: flex;
  gap: 2px;
  margin-top: 4px;
}

.x-label {
  text-align: center;
  font-size: 0.55rem;
  font-weight: 600;
  color: #888;
}

.x-axis-title {
  text-align: center;
  font-size: 0.65rem;
  color: #999;
  margin-top: 0.3rem;
}

.risk-legend {
  margin-top: 0.8rem;
  font-size: 0.65rem;
  color: #666;
  line-height: 1.8;
}
</style>
