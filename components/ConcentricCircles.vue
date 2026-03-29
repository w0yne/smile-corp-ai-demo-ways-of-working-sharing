<script setup>
defineProps({
  circles: {
    type: Array,
    required: true
    // From outermost to innermost: { label: string, sublabel?: string, size: number, color: string, textColor?: string, labelPosition?: string }
  },
  width: { type: Number, default: 380 },
  height: { type: Number, default: 380 }
})
</script>

<template>
  <div class="concentric-circles">
    <div class="circles-container" :style="{ width: `${width}px`, height: `${height}px` }">
      <div
        v-for="(circle, idx) in circles"
        :key="idx"
        class="circle"
        :style="{
          width: `${circle.size}px`,
          height: `${circle.size}px`,
          backgroundColor: `${circle.color}15`,
          borderColor: circle.color
        }"
      >
        <div
          v-if="idx === circles.length - 1"
          class="center-label"
          :style="{ color: circle.textColor || circle.color }"
        >
          <div class="center-title">{{ circle.label }}</div>
          <div v-if="circle.sublabel" class="center-sublabel">{{ circle.sublabel }}</div>
        </div>
      </div>

      <!-- Labels for outer circles -->
      <template v-for="(circle, idx) in circles" :key="`label-${idx}`">
        <div
          v-if="idx < circles.length - 1 && circle.labelPosition"
          class="outer-label"
          :style="getLabelStyle(circle)"
        >
          <div class="outer-title" :style="{ color: circle.textColor || circle.color }">{{ circle.label }}</div>
          <div v-if="circle.sublabel" class="outer-sublabel">{{ circle.sublabel }}</div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    getLabelStyle(circle) {
      const positions = {
        'top': { top: '18%', left: '50%', transform: 'translateX(-50%)' },
        'top-right': { top: '32%', right: '12%' },
        'bottom': { bottom: '5%', left: '50%', transform: 'translateX(-50%)' }
      }
      return positions[circle.labelPosition] || {}
    }
  }
}
</script>

<style scoped>
.concentric-circles {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 320px;
}

.circles-container {
  position: relative;
}

.circle {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  border: 2px solid;
  display: flex;
  align-items: center;
  justify-content: center;
}

.center-label {
  text-align: center;
}

.center-title {
  font-size: 0.75rem;
  font-weight: 700;
}

.center-sublabel {
  font-size: 0.55rem;
  color: #888;
}

.outer-label {
  position: absolute;
  text-align: center;
}

.outer-title {
  font-size: 0.65rem;
  font-weight: 700;
}

.outer-sublabel {
  font-size: 0.5rem;
  color: #888;
}
</style>
