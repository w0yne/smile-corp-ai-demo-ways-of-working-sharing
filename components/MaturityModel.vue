<script setup>
defineProps({
  levels: {
    type: Array,
    required: true
    // Each: { number: number, label: string, sublabel?: string }
  },
  dimensions: {
    type: Array,
    required: true
    // Each: { label: string, currentLevel: number, color: string }
  }
})
</script>

<template>
  <div class="maturity-model">
    <div class="grid">
      <!-- Header row -->
      <div class="header-row">
        <div class="dimension-header"></div>
        <div
          v-for="level in levels"
          :key="level.number"
          class="level-header"
        >
          Level {{ level.number }}<br />
          <span class="level-sublabel">{{ level.sublabel }}</span>
        </div>
      </div>

      <!-- Dimension rows -->
      <div
        v-for="dim in dimensions"
        :key="dim.label"
        class="dimension-row"
      >
        <div class="dimension-label">{{ dim.label }}</div>
        <div
          v-for="level in levels"
          :key="level.number"
          class="level-cell"
          :style="{
            backgroundColor: dim.currentLevel === level.number ? dim.color : '#e5e7eb'
          }"
        >
          <span v-if="dim.currentLevel === level.number">●</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.maturity-model {
  margin: 1rem auto;
  max-width: 700px;
}

.grid {
  font-size: 0.7rem;
}

.header-row,
.dimension-row {
  display: grid;
  grid-template-columns: 100px repeat(5, 1fr);
  gap: 2px;
}

.dimension-header,
.level-header {
  padding: 0.4rem;
  text-align: center;
  font-weight: 600;
  color: #888;
}

.level-sublabel {
  font-size: 0.55rem;
  font-weight: 400;
}

.dimension-row {
  border-top: 1px solid #e5e7eb;
}

.dimension-label {
  padding: 0.5rem;
  font-weight: 500;
}

.level-cell {
  padding: 0.5rem;
  border-radius: 4px;
  text-align: center;
  color: white;
  font-weight: 600;
}
</style>
