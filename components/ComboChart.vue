<script setup>
defineProps({
  bars: {
    type: Array,
    required: true
    // Each bar: { value: string, height: number, color: string }
  },
  linePath: { type: String, required: true },
  linePoints: {
    type: Array,
    default: () => []
    // Each: { cx: number, cy: number, label?: string }
  },
  labels: {
    type: Array,
    default: () => []
  },
  legend: {
    type: Array,
    default: () => []
    // Each: { label: string, color: string, type: 'bar' | 'line' }
  },
  height: { type: Number, default: 260 },
  barColor: { type: String, default: '#0284c7' },
  lineColor: { type: String, default: '#ef4444' }
})
</script>

<template>
  <div class="combo-chart">
    <div class="chart-area" :style="{ height: `${height}px` }">
      <div class="bars-container">
        <div v-for="(bar, idx) in bars" :key="idx" class="bar-item">
          <div class="bar-value">{{ bar.value }}</div>
          <div
            class="bar"
            :style="{
              width: '55px',
              height: `${bar.height}px`,
              backgroundColor: bar.color || barColor
            }"
          ></div>
        </div>
      </div>

      <!-- Line overlay -->
      <svg viewBox="0 0 500 260" class="line-overlay">
        <path :d="linePath" fill="none" :stroke="lineColor" stroke-width="3"/>
        <template v-for="(pt, idx) in linePoints" :key="idx">
          <circle :cx="pt.cx" :cy="pt.cy" r="6" :fill="lineColor" stroke="white" stroke-width="2"/>
          <text
            v-if="pt.label"
            :x="pt.cx + 5"
            :y="pt.cy - 5"
            style="font-size:11px"
            :fill="lineColor"
            font-weight="600"
          >{{ pt.label }}</text>
        </template>
      </svg>
    </div>

    <!-- X-axis labels -->
    <div v-if="labels.length > 0" class="x-labels">
      <span v-for="(label, idx) in labels" :key="idx">{{ label }}</span>
    </div>

    <!-- Legend -->
    <div v-if="legend.length > 0" class="legend">
      <div v-for="(item, idx) in legend" :key="idx" class="legend-item">
        <div
          class="legend-indicator"
          :class="item.type"
          :style="{ backgroundColor: item.color }"
        ></div>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.combo-chart {
  width: 100%;
}

.chart-area {
  position: relative;
  margin: 1.5rem 2rem 0;
}

.bars-container {
  display: flex;
  align-items: flex-end;
  justify-content: space-around;
  height: 100%;
  padding: 0 1rem;
}

.bar-item {
  text-align: center;
  z-index: 1;
}

.bar-value {
  font-size: 0.6rem;
  color: #888;
  margin-bottom: 0.2rem;
}

.bar {
  border-radius: 4px 4px 0 0;
  opacity: 0.8;
}

.line-overlay {
  position: absolute;
  top: 0;
  left: 20px;
  width: calc(100% - 20px);
  height: 100%;
  pointer-events: none;
}

.x-labels {
  display: flex;
  justify-content: space-between;
  margin: 0.5rem 2rem 0;
  padding-top: 0.3rem;
  border-top: 1px solid #e5e7eb;
}

.x-labels span {
  font-size: 0.75rem;
  color: #888;
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
  border-radius: 2px;
}

.legend-indicator.bar {
  width: 12px;
  height: 12px;
}

.legend-indicator.line {
  width: 16px;
  height: 3px;
}

.legend-item span {
  font-size: 0.7rem;
  color: #888;
}
</style>
