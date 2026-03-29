<script setup>
defineProps({
  value: { type: Number, required: true },
  label: { type: String, default: '' },
  sublabel: { type: String, default: '' },
  color: { type: String, default: '#16a34a' },
  size: { type: Number, default: 180 }
})
</script>

<template>
  <div class="gauge-chart">
    <div class="gauge-container" :style="{ width: `${size}px`, height: `${size / 2}px` }">
      <!-- Background arc -->
      <div
        class="gauge-bg"
        :style="{
          width: `${size}px`,
          height: `${size}px`,
          borderColor: '#e5e7eb'
        }"
      ></div>
      <!-- Foreground arc - simplified visual -->
      <div
        class="gauge-fg"
        :style="{
          width: `${size}px`,
          height: `${size}px`,
          borderColor: color,
          transform: `rotate(${225 + (value / 100) * 180}deg)`
        }"
      ></div>
      <!-- Value display -->
      <div class="gauge-value" :style="{ color }">{{ value }}%</div>
    </div>
    <div v-if="label" class="gauge-label">{{ label }}</div>
    <div v-if="sublabel" class="gauge-sublabel">{{ sublabel }}</div>
  </div>
</template>

<style scoped>
.gauge-chart {
  text-align: center;
}

.gauge-container {
  position: relative;
  margin: 0 auto;
  overflow: hidden;
}

.gauge-bg,
.gauge-fg {
  position: absolute;
  top: 0;
  left: 0;
  border-radius: 50%;
  border-width: 16px;
  border-style: solid;
  border-bottom-color: transparent;
  border-left-color: transparent;
  box-sizing: border-box;
}

.gauge-bg {
  transform: rotate(225deg);
}

.gauge-fg {
  border-right-color: transparent;
  transform-origin: center center;
}

.gauge-value {
  position: absolute;
  bottom: 5px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 2rem;
  font-weight: 700;
}

.gauge-label {
  font-size: 0.8rem;
  font-weight: 600;
  margin-top: 0.5rem;
}

.gauge-sublabel {
  font-size: 0.7rem;
  color: #888;
}
</style>
