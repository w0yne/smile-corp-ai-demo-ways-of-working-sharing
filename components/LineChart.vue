<script setup>
defineProps({
  points: {
    type: Array,
    required: true
    // Each point: { x: number (percentage), y: number (percentage from bottom), value: string }
  },
  labels: {
    type: Array,
    default: () => []
    // X-axis labels
  },
  annotation: {
    type: Object,
    default: null
    // { title: string, value: string, subtitle?: string }
  },
  height: { type: Number, default: 280 },
  color: { type: String, default: '#0284c7' },
  showGridLines: { type: Boolean, default: true }
})
</script>

<template>
  <div class="line-chart">
    <div class="chart-area" :style="{ height: `${height}px` }">
      <!-- Grid lines -->
      <template v-if="showGridLines">
        <div class="grid-line" style="bottom: 0;"></div>
        <div class="grid-line dashed" style="bottom: 25%;"></div>
        <div class="grid-line dashed" style="bottom: 50%;"></div>
        <div class="grid-line dashed" style="bottom: 75%;"></div>
      </template>

      <!-- Connecting line (SVG) -->
      <svg class="line-svg" viewBox="0 0 100 100" preserveAspectRatio="none">
        <polyline
          :points="points.map(p => `${p.x},${100 - p.y}`).join(' ')"
          :stroke="color"
          stroke-width="0.8"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>

      <!-- Data points -->
      <div
        v-for="(point, idx) in points"
        :key="idx"
        class="point"
        :style="{
          left: `${point.x}%`,
          bottom: `${point.y}%`,
          backgroundColor: color,
          boxShadow: `0 0 0 1px ${color}`
        }"
      ></div>

      <!-- Value labels -->
      <div
        v-for="(point, idx) in points"
        :key="`label-${idx}`"
        class="point-label"
        :style="{
          left: `${point.x - 3}%`,
          bottom: `${point.y + 4}%`,
          color: color
        }"
      >
        {{ point.value }}
      </div>

      <!-- Annotation box -->
      <div v-if="annotation" class="annotation">
        <div class="annotation-title">{{ annotation.title }}</div>
        <div class="annotation-value" :style="{ color }">{{ annotation.value }}</div>
        <div v-if="annotation.subtitle" class="annotation-subtitle">{{ annotation.subtitle }}</div>
      </div>
    </div>

    <!-- X-axis labels -->
    <div v-if="labels.length > 0" class="x-labels">
      <span v-for="(label, idx) in labels" :key="idx">{{ label }}</span>
    </div>
  </div>
</template>

<style scoped>
.line-chart {
  width: 100%;
}

.chart-area {
  position: relative;
  margin: 1.5rem 2rem 0;
}

.grid-line {
  position: absolute;
  left: 0;
  right: 0;
  border-top: 1px solid #e5e7eb;
  height: 0;
}

.grid-line.dashed {
  border-top-style: dashed;
  border-color: #f1f5f9;
}

.line-svg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: visible;
}

.point {
  position: absolute;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  border: 3px solid white;
  transform: translate(-50%, 50%);
}

.point-label {
  position: absolute;
  font-size: 0.7rem;
  font-weight: 600;
}

.annotation {
  position: absolute;
  top: 10%;
  right: 2%;
  padding: 0.5rem 0.8rem;
  background: #f0f9ff;
  border: 1px solid #bae6fd;
  border-radius: 6px;
  text-align: center;
}

.annotation-title {
  font-size: 0.65rem;
  color: #999;
  text-transform: uppercase;
}

.annotation-value {
  font-size: 1.2rem;
  font-weight: 700;
}

.annotation-subtitle {
  font-size: 0.6rem;
  color: #888;
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
