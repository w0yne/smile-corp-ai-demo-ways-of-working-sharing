<script setup>
defineProps({
  circles: {
    type: Array,
    required: true
    // Each: { label: string, position: { top, left, right, bottom }, size: number, color: string }
  },
  centerLabel: {
    type: [Object, String],
    default: null
    // string OR { text: string, sublabel?: string }
  },
  width: { type: Number, default: 400 },
  height: { type: Number, default: 350 }
})
</script>

<template>
  <div class="venn-diagram">
    <div class="diagram-container" :style="{ width: `${width}px`, height: `${height}px` }">
      <div
        v-for="(circle, idx) in circles"
        :key="idx"
        class="venn-circle"
        :style="{
          ...circle.position,
          width: `${circle.size}px`,
          height: `${circle.size}px`,
          backgroundColor: `${circle.color}18`,
          borderColor: circle.color
        }"
      ></div>

      <!-- Circle labels -->
      <div
        v-for="(circle, idx) in circles"
        :key="`label-${idx}`"
        class="circle-label"
        :style="circle.labelPosition"
      >
        <span :style="{ color: circle.color }">{{ circle.label }}</span>
      </div>

      <!-- Center label -->
      <div v-if="centerLabel" class="center-label">
        <span class="center-text">{{ typeof centerLabel === 'string' ? centerLabel : centerLabel.text }}</span>
        <span v-if="typeof centerLabel === 'object' && centerLabel.sublabel" class="center-sublabel">{{ centerLabel.sublabel }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.venn-diagram {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 320px;
}

.diagram-container {
  position: relative;
}

.venn-circle {
  position: absolute;
  border-radius: 50%;
  border: 2px solid;
}

.circle-label {
  position: absolute;
  font-size: 0.75rem;
  font-weight: 600;
  line-height: 1.3;
}

.center-label {
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -20%);
  background: #111;
  color: white;
  padding: 0.4rem 0.8rem;
  border-radius: 6px;
  font-size: 0.7rem;
  font-weight: 700;
  text-align: center;
  z-index: 10;
}

.center-text {
  display: block;
}

.center-sublabel {
  display: block;
  font-size: 0.6rem;
  font-weight: 400;
}
</style>
