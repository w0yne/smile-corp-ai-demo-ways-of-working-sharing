<script setup>
defineProps({
  months: {
    type: Array,
    required: true
    // Single-letter month labels
  },
  tasks: {
    type: Array,
    required: true
    // Each: { label: string, critical?: boolean, bars: [{ start: number, span: number, color: string }] }
  },
  legend: {
    type: Array,
    default: () => []
  },
  labelWidth: { type: Number, default: 140 }
})
</script>

<template>
  <div class="gantt-chart">
    <div class="grid-container">
      <!-- Header row -->
      <div class="header-row">
        <div class="task-label-header" :style="{ width: `${labelWidth}px` }">Task</div>
        <div
          v-for="(month, idx) in months"
          :key="idx"
          class="month-header"
        >
          {{ month }}
        </div>
      </div>

      <!-- Task rows -->
      <div
        v-for="(task, tIdx) in tasks"
        :key="tIdx"
        class="task-row"
      >
        <div
          class="task-label"
          :style="{ width: `${labelWidth}px` }"
          :class="{ critical: task.critical }"
        >
          <span v-if="task.critical">▸ </span>{{ task.label }}
        </div>
        <div class="task-timeline" :style="{ gridTemplateColumns: `repeat(${months.length}, 1fr)` }">
          <!-- Bars placed by grid column -->
          <template v-for="(bar, bIdx) in task.bars" :key="bIdx">
            <div
              v-if="bar.span > 0"
              class="task-bar"
              :style="{
                gridColumn: `${bar.start + 1} / span ${bar.span}`,
                backgroundColor: bar.color
              }"
            ></div>
          </template>
        </div>
      </div>
    </div>

    <!-- Legend -->
    <div v-if="legend.length > 0" class="legend">
      <div v-for="(item, idx) in legend" :key="idx" class="legend-item">
        <div class="legend-color" :style="{ backgroundColor: item.color }"></div>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.gantt-chart {
  margin: 1rem auto;
  max-width: 700px;
}

.grid-container {
  font-size: 0.6rem;
}

.header-row {
  display: flex;
  gap: 1px;
}

.task-label-header {
  padding: 0.3rem;
  font-weight: 600;
  font-size: 0.7rem;
}

.month-header {
  flex: 1;
  text-align: center;
  padding: 0.3rem;
  color: #888;
}

.task-row {
  display: flex;
  gap: 1px;
  border-top: 1px solid #e5e7eb;
}

.task-label {
  padding: 0.4rem;
  font-size: 0.65rem;
}

.task-label.critical {
  font-weight: 600;
  color: #ef4444;
}

.task-timeline {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 2px;
  min-height: 24px;
  align-items: center;
  background: repeating-linear-gradient(
    to right,
    transparent,
    transparent calc(100% / 12 - 1px),
    #e5e7eb calc(100% / 12 - 1px),
    #e5e7eb calc(100% / 12)
  );
}

.task-bar {
  border-radius: 3px;
  height: 16px;
}

.legend {
  display: flex;
  gap: 1.5rem;
  margin-top: 0.8rem;
  justify-content: center;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
}

.legend-color {
  width: 16px;
  height: 8px;
  border-radius: 2px;
}

.legend-item span {
  font-size: 0.65rem;
  color: #888;
}
</style>
