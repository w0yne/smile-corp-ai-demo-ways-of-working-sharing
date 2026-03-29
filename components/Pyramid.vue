<script setup>
defineProps({
  levels: {
    type: Array,
    required: true
    // Each level from top to bottom: { label: string, sublabel?: string, width: string, color: string, textColor?: string }
  },
  annotations: {
    type: Array,
    default: () => []
    // Each: { label: string, sublabel?: string, color: string }
  }
})
</script>

<template>
  <div class="pyramid">
    <div class="pyramid-stack">
      <div
        v-for="(level, idx) in levels"
        :key="idx"
        class="level"
        :class="{ 'is-top': idx === 0 }"
        :style="{
          width: level.width,
          backgroundColor: level.color,
          color: level.textColor || 'white',
          clipPath: idx === 0 ? 'polygon(50% 0%, 100% 100%, 0% 100%)' : 'none'
        }"
      >
        <div class="level-content" :style="{ paddingTop: idx === 0 ? '2rem' : '0' }">
          <div class="level-label">{{ level.label }}</div>
          <div v-if="level.sublabel" class="level-sublabel">{{ level.sublabel }}</div>
        </div>
      </div>
    </div>

    <div v-if="annotations.length > 0" class="annotations">
      <div
        v-for="(ann, idx) in annotations"
        :key="idx"
        class="annotation"
      >
        <div class="annotation-label" :style="{ color: ann.color }">{{ ann.label }}</div>
        <div v-if="ann.sublabel" class="annotation-sublabel">{{ ann.sublabel }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pyramid {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 2rem;
}

.pyramid-stack {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.level {
  text-align: center;
  margin-top: -1px;
}

.level.is-top {
  height: 100px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  padding-bottom: 0.8rem;
}

.level:not(.is-top) {
  padding: 1rem 2rem;
}

.level-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.level-label {
  font-size: 0.8rem;
  font-weight: 700;
}

.level-sublabel {
  font-size: 0.6rem;
  opacity: 0.8;
  margin-top: 0.3rem;
  line-height: 1.4;
}

.annotations {
  display: flex;
  justify-content: center;
  gap: 3rem;
  margin-top: 2rem;
}

.annotation {
  text-align: center;
}

.annotation-label {
  font-size: 1.5rem;
  font-weight: 700;
}

.annotation-sublabel {
  font-size: 0.7rem;
  color: #888;
}
</style>
