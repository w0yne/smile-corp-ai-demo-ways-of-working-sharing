<script setup>
defineProps({
  quadrants: {
    type: Array,
    default: () => []
    // Each: { position: 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right', label: string, color?: string, style?: string }
  },
  bubbles: {
    type: Array,
    required: true
    // Each: { x: string (CSS), y: string (CSS), size: number, label: string, color: string }
  },
  xAxisLabel: { type: String, default: '' },
  yAxisLabel: { type: String, default: '' },
  height: { type: Number, default: 300 }
})
</script>

<template>
  <div class="scatter-quadrant">
    <div class="chart-area" :style="{ height: `${height}px` }">
      <!-- Axis lines -->
      <div class="vertical-axis"></div>
      <div class="horizontal-axis"></div>

      <!-- Quadrant labels -->
      <div
        v-for="(q, idx) in quadrants"
        :key="idx"
        class="quadrant-label"
        :class="q.position"
        :style="{ color: q.color || '#999', fontStyle: q.style || 'normal', fontWeight: q.fontWeight || 'normal' }"
      >
        {{ q.label }}
      </div>

      <!-- Bubbles -->
      <div
        v-for="(bubble, idx) in bubbles"
        :key="`bubble-${idx}`"
        class="bubble"
        :style="{
          left: bubble.x,
          top: bubble.y,
          width: `${bubble.size}px`,
          height: `${bubble.size}px`,
          backgroundColor: `${bubble.color}20`,
          borderColor: bubble.color
        }"
      >
        <span :style="{ color: bubble.color }">{{ bubble.label }}</span>
      </div>

      <!-- Axis labels -->
      <div v-if="xAxisLabel" class="x-axis-label">{{ xAxisLabel }}</div>
      <div v-if="yAxisLabel" class="y-axis-label">{{ yAxisLabel }}</div>
    </div>
  </div>
</template>

<style scoped>
.scatter-quadrant {
  width: 100%;
}

.chart-area {
  position: relative;
  margin: 0.5rem 2rem 0;
}

.vertical-axis {
  position: absolute;
  left: 50%;
  top: 0;
  bottom: 0;
  border-left: 1px dashed #d1d5db;
}

.horizontal-axis {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  border-top: 1px dashed #d1d5db;
}

.quadrant-label {
  position: absolute;
  font-size: 0.7rem;
}

.quadrant-label.top-left {
  top: 8%;
  left: 8%;
}

.quadrant-label.top-right {
  top: 8%;
  right: 8%;
}

.quadrant-label.bottom-left {
  bottom: 8%;
  left: 8%;
}

.quadrant-label.bottom-right {
  bottom: 8%;
  right: 8%;
}

.bubble {
  position: absolute;
  border-radius: 50%;
  border: 2px solid;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: translate(-50%, -50%);
}

.bubble span {
  font-size: 0.6rem;
  font-weight: 600;
  text-align: center;
}

.x-axis-label {
  position: absolute;
  bottom: -0.5rem;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.7rem;
  color: #999;
}

.y-axis-label {
  position: absolute;
  top: 50%;
  left: -1.5rem;
  transform: rotate(-90deg) translateX(-50%);
  font-size: 0.7rem;
  color: #999;
  transform-origin: left center;
}
</style>
