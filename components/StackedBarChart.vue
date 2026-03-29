<script setup>
defineProps({
  bars: {
    type: Array,
    required: true
    // Each bar: { label: string, total?: string, segments: [{ value: number | string, color: string, showLabel?: boolean }] }
  },
  legend: {
    type: Array,
    default: () => []
    // Each: { label: string, color: string }
  },
  height: { type: Number, default: 260 },
  barWidth: { type: Number, default: 90 },
  variant: { type: String, default: 'absolute' } // 'absolute' | 'percentage'
})
</script>

<template>
  <div class="stacked-bar-chart">
    <div class="chart-area" :style="{ height: `${height}px` }">
      <div v-for="(bar, idx) in bars" :key="idx" class="bar-wrapper">
        <div v-if="bar.total" class="bar-total">{{ bar.total }}</div>
        <div class="bar-stack" :style="{ width: `${barWidth}px`, height: variant === 'percentage' ? `${height - 20}px` : 'auto' }">
          <div
            v-for="(seg, sIdx) in bar.segments"
            :key="sIdx"
            class="segment"
            :style="{
              height: variant === 'percentage' ? `${seg.value}%` : `${seg.value}px`,
              backgroundColor: seg.color,
              borderRadius: sIdx === 0 ? '4px 4px 0 0' : sIdx === bar.segments.length - 1 ? '0 0 4px 4px' : '0'
            }"
          >
            <span v-if="seg.showLabel" class="segment-label" :style="{ color: seg.labelColor || 'white' }">
              {{ typeof seg.value === 'number' && variant === 'percentage' ? `${seg.value}%` : seg.label || '' }}
            </span>
          </div>
        </div>
        <div class="bar-label">{{ bar.label }}</div>
      </div>
    </div>
    <div v-if="legend.length > 0" class="legend">
      <div v-for="(item, idx) in legend" :key="idx" class="legend-item">
        <div class="legend-color" :style="{ backgroundColor: item.color }"></div>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.stacked-bar-chart {
  width: 100%;
  margin-top: 0.5rem;
}

.chart-area {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 2rem;
  overflow: hidden;
}

.bar-wrapper {
  text-align: center;
}

.bar-total {
  font-size: 0.7rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
}

.bar-stack {
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border-radius: 4px;
}

.segment {
  display: flex;
  align-items: center;
  justify-content: center;
}

.segment-label {
  font-size: 0.6rem;
  font-weight: 600;
}

.bar-label {
  font-size: 0.75rem;
  color: #888;
  margin-top: 0.3rem;
}

.legend {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-top: 1rem;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 2px;
}

.legend-item span {
  font-size: 0.7rem;
  color: #888;
}
</style>
