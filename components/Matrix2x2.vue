<script setup>
defineProps({
  cells: {
    type: Array,
    required: true
    // 4 cells: [topLeft, topRight, bottomLeft, bottomRight]
    // Each: { title: string, subtitle?: string, color: string, bgColor: string, items?: string[] }
  },
  xAxisLabel: { type: String, default: '' },
  yAxisLabel: { type: String, default: '' },
  width: { type: Number, default: 520 },
  height: { type: Number, default: 340 }
})
</script>

<template>
  <div class="matrix-2x2" :style="{ width: `${width}px`, height: `${height}px` }">
    <div class="grid">
      <div
        v-for="(cell, idx) in cells"
        :key="idx"
        class="cell"
        :style="{ backgroundColor: cell.bgColor }"
      >
        <div class="cell-header">
          <span class="cell-title" :style="{ color: cell.color }">{{ cell.title }}</span>
          <span v-if="cell.star" class="cell-star"> ★</span>
        </div>
        <div v-if="cell.subtitle" class="cell-subtitle">{{ cell.subtitle }}</div>
        <div v-if="cell.items && cell.items.length > 0" class="cell-items">
          <div
            v-for="(item, iIdx) in cell.items"
            :key="iIdx"
            class="cell-item"
            :style="{ borderColor: cell.itemBorderColor || cell.color + '40' }"
          >
            {{ item }}
          </div>
        </div>
        <slot :name="`cell-${idx}`"></slot>
      </div>
    </div>
    <div v-if="xAxisLabel" class="x-axis-label">{{ xAxisLabel }}</div>
    <div v-if="yAxisLabel" class="y-axis-label">{{ yAxisLabel }}</div>
  </div>
</template>

<style scoped>
.matrix-2x2 {
  position: relative;
  margin: 1rem auto 0;
  margin-left: 60px;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  height: calc(100% - 40px);
  gap: 2px;
  background: #e5e7eb;
  border-radius: 8px;
  overflow: hidden;
}

.cell {
  padding: 1rem;
  position: relative;
}

.cell-header {
  display: flex;
  align-items: center;
}

.cell-title {
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.cell-star {
  color: inherit;
}

.cell-subtitle {
  font-size: 0.65rem;
  color: #888;
  margin-top: 0.2rem;
}

.cell-items {
  margin-top: 0.6rem;
}

.cell-item {
  font-size: 0.7rem;
  padding: 0.2rem 0.4rem;
  background: white;
  border-radius: 4px;
  margin-bottom: 0.3rem;
  border: 1px solid;
}

.x-axis-label {
  position: absolute;
  bottom: -20px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.7rem;
  color: #999;
}

.y-axis-label {
  position: absolute;
  top: 50%;
  left: -50px;
  transform: translateY(-50%) rotate(-90deg);
  font-size: 0.7rem;
  color: #999;
  white-space: nowrap;
}
</style>
