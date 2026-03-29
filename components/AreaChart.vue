<script setup>
defineProps({
  svgPath: { type: String, required: true },
  fillPath: { type: String, required: true },
  points: {
    type: Array,
    default: () => []
    // Each: { cx: number, cy: number }
  },
  labels: {
    type: Array,
    default: () => []
    // X-axis labels
  },
  valueLabels: {
    type: Array,
    default: () => []
    // Each: { x: string (CSS), y: string (CSS), value: string }
  },
  height: { type: Number, default: 260 },
  color: { type: String, default: '#0284c7' },
  showGridLines: { type: Boolean, default: true },
  gridLabels: {
    type: Array,
    default: () => []
    // Each: { y: string (percentage), label: string }
  }
})
</script>

<template>
  <div class="area-chart">
    <div
      class="chart-area"
      :style="{
        height: `${height}px`,
        background: `linear-gradient(to top, rgba(2,132,199,0.08), rgba(2,132,199,0.02))`
      }"
    >
      <!-- Grid lines -->
      <template v-if="showGridLines">
        <div
          v-for="(gl, idx) in gridLabels"
          :key="idx"
          class="grid-line"
          :style="{ bottom: gl.y }"
        >
          <span class="grid-label">{{ gl.label }}</span>
        </div>
      </template>

      <!-- SVG Area -->
      <svg viewBox="0 0 500 260" class="chart-svg">
        <defs>
          <linearGradient id="areaGradient" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" :stop-color="color" stop-opacity="0.3"/>
            <stop offset="100%" :stop-color="color" stop-opacity="0.02"/>
          </linearGradient>
        </defs>
        <path :d="fillPath" fill="url(#areaGradient)"/>
        <path :d="svgPath" fill="none" :stroke="color" stroke-width="3"/>
        <circle
          v-for="(pt, idx) in points"
          :key="idx"
          :cx="pt.cx"
          :cy="pt.cy"
          r="5"
          :fill="color"
        />
      </svg>

      <!-- Value labels -->
      <div
        v-for="(vl, idx) in valueLabels"
        :key="`vl-${idx}`"
        class="value-label"
        :style="{ left: vl.x, bottom: vl.y, top: vl.top }"
      >
        {{ vl.value }}
      </div>
    </div>

    <!-- X-axis labels -->
    <div v-if="labels.length > 0" class="x-labels">
      <span v-for="(label, idx) in labels" :key="idx">{{ label }}</span>
    </div>
  </div>
</template>

<style scoped>
.area-chart {
  width: 100%;
}

.chart-area {
  position: relative;
  margin: 1.5rem 2rem 0;
  border-radius: 0 0 8px 8px;
}

.grid-line {
  position: absolute;
  left: 0;
  right: 0;
  border-top: 1px dashed #e5e7eb;
}

.grid-label {
  position: absolute;
  left: 0;
  font-size: 0.6rem;
  color: #bbb;
  transform: translateY(-50%);
}

.chart-svg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.value-label {
  position: absolute;
  font-size: 0.65rem;
  color: #0284c7;
  font-weight: 600;
}

.x-labels {
  display: flex;
  justify-content: space-between;
  margin: 0.5rem 2rem 0;
  padding-top: 0.5rem;
  border-top: 1px solid #e5e7eb;
}

.x-labels span {
  font-size: 0.75rem;
  color: #888;
}
</style>
