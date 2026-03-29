<script setup>
defineProps({
  columns: {
    type: Array,
    required: true
    // Each column: { width: string (e.g. '40%'), label: string, segments: [{ flex: number, color: string, label: string, labelColor?: string }] }
  },
  height: { type: Number, default: 260 }
})
</script>

<template>
  <div class="marimekko-chart">
    <div class="chart-area" :style="{ height: `${height}px` }">
      <div
        v-for="(col, cIdx) in columns"
        :key="cIdx"
        class="column"
        :style="{ width: col.width }"
      >
        <div
          v-for="(seg, sIdx) in col.segments"
          :key="sIdx"
          class="segment"
          :style="{
            flex: seg.flex,
            backgroundColor: seg.color,
            borderRight: cIdx < columns.length - 1 ? '1px solid white' : 'none',
            borderBottom: sIdx < col.segments.length - 1 ? '1px solid white' : 'none'
          }"
        >
          <span :style="{ color: seg.labelColor || 'white' }">{{ seg.label }}</span>
        </div>
      </div>
    </div>
    <div class="labels">
      <div
        v-for="(col, idx) in columns"
        :key="idx"
        class="column-label"
        :style="{ width: col.width }"
      >
        {{ col.label }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.marimekko-chart {
  margin: 1rem 1rem 0;
}

.chart-area {
  display: flex;
  border: 1px solid #e5e7eb;
  border-radius: 4px;
  overflow: hidden;
}

.column {
  display: flex;
  flex-direction: column;
}

.segment {
  display: flex;
  align-items: center;
  justify-content: center;
}

.segment span {
  font-size: 0.65rem;
  font-weight: 600;
  text-align: center;
}

.labels {
  display: flex;
  margin-top: 0.3rem;
}

.column-label {
  text-align: center;
  font-size: 0.7rem;
  color: #888;
}
</style>
