<script setup>
defineProps({
  dimensions: {
    type: Array,
    required: true
    // Each: { label: string, position: object (CSS) }
  },
  rings: {
    type: Array,
    default: () => [60, 120, 180, 240]
    // Ring sizes from center outward
  },
  polygons: {
    type: Array,
    required: true
    // Each: { points: string (SVG points), color: string, dashed?: boolean, fill?: boolean }
  },
  legend: {
    type: Array,
    default: () => []
  },
  size: { type: Number, default: 300 }
})
</script>

<template>
  <div class="radar-chart">
    <div class="chart-container" :style="{ width: `${size}px`, height: `${size}px` }">
      <!-- Concentric rings -->
      <div
        v-for="(ringSize, idx) in rings"
        :key="`ring-${idx}`"
        class="ring"
        :style="{
          width: `${ringSize}px`,
          height: `${ringSize}px`
        }"
      ></div>

      <!-- SVG polygons -->
      <svg :viewBox="`0 0 ${size} ${size}`" class="polygon-svg">
        <polygon
          v-for="(poly, idx) in polygons"
          :key="idx"
          :points="poly.points"
          :fill="poly.fill ? `${poly.color}15` : 'none'"
          :stroke="poly.color"
          stroke-width="2"
          :stroke-dasharray="poly.dashed ? '6,3' : '0'"
        />
      </svg>

      <!-- Dimension labels -->
      <div
        v-for="(dim, idx) in dimensions"
        :key="`dim-${idx}`"
        class="dimension-label"
        :style="dim.position"
      >
        {{ dim.label }}
      </div>
    </div>

    <!-- Legend -->
    <div v-if="legend.length > 0" class="legend">
      <div v-for="(item, idx) in legend" :key="idx" class="legend-item">
        <div
          class="legend-indicator"
          :style="{
            backgroundColor: item.fill ? item.color : 'transparent',
            borderColor: item.color,
            borderStyle: item.dashed ? 'dashed' : 'solid'
          }"
        ></div>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.radar-chart {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.chart-container {
  position: relative;
}

.ring {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid #e5e7eb;
  border-radius: 50%;
}

.polygon-svg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.dimension-label {
  position: absolute;
  font-size: 0.7rem;
  font-weight: 600;
  color: #333;
}

.legend {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-top: 0.5rem;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
}

.legend-indicator {
  width: 16px;
  height: 3px;
  border-radius: 2px;
  border-width: 2px;
}

.legend-item span {
  font-size: 0.7rem;
  color: #888;
}
</style>
