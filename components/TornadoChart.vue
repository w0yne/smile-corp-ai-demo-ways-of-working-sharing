<script setup>
defineProps({
  rows: {
    type: Array,
    required: true
    // Each row: { label: string, leftWidth: number (percentage), leftValue: string, rightWidth: number (percentage), rightValue: string }
  },
  centerLabel: { type: String, default: '' },
  leftLabel: { type: String, default: 'Downside' },
  rightLabel: { type: String, default: 'Upside' },
  leftColor: { type: String, default: '#ef4444' },
  rightColor: { type: String, default: '#16a34a' },
  maxWidth: { type: Number, default: 650 },
  labelWidth: { type: Number, default: 100 },
  barHeight: { type: Number, default: 28 }
})
</script>

<template>
  <div class="tornado-chart" :style="{ maxWidth: `${maxWidth}px` }">
    <div class="header">
      <span>← {{ leftLabel }}</span>
      <span v-if="centerLabel">{{ centerLabel }}</span>
      <span>{{ rightLabel }} →</span>
    </div>
    <div class="center-line"></div>

    <div v-for="(row, idx) in rows" :key="idx" class="row">
      <div class="row-label" :style="{ width: `${labelWidth}px` }">{{ row.label }}</div>
      <div class="bars-container" :style="{ height: `${barHeight}px` }">
        <div class="left-side">
          <div
            class="bar left-bar"
            :style="{
              width: `${row.leftWidth}%`,
              backgroundColor: leftColor
            }"
          >
            <span>{{ row.leftValue }}</span>
          </div>
        </div>
        <div class="right-side">
          <div
            class="bar right-bar"
            :style="{
              width: `${row.rightWidth}%`,
              backgroundColor: rightColor
            }"
          >
            <span>{{ row.rightValue }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tornado-chart {
  margin: 1.5rem auto 0;
}

.header {
  text-align: center;
  font-size: 0.7rem;
  color: #999;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: space-between;
  padding-left: 100px;
}

.center-line {
  position: relative;
  border-left: 2px dashed #d1d5db;
  margin-left: calc(50% + 50px);
  height: 0;
  margin-bottom: 0.5rem;
}

.row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.6rem;
}

.row:last-child {
  margin-bottom: 0;
}

.row-label {
  text-align: right;
  font-size: 0.75rem;
  font-weight: 500;
}

.bars-container {
  flex: 1;
  display: flex;
}

.left-side,
.right-side {
  width: 50%;
  display: flex;
}

.left-side {
  justify-content: flex-end;
}

.right-side {
  justify-content: flex-start;
}

.bar {
  height: 100%;
  display: flex;
  align-items: center;
}

.left-bar {
  border-radius: 4px 0 0 4px;
  padding-left: 0.3rem;
}

.right-bar {
  border-radius: 0 4px 4px 0;
  justify-content: flex-end;
  padding-right: 0.3rem;
}

.bar span {
  font-size: 0.6rem;
  color: white;
  font-weight: 600;
}
</style>
