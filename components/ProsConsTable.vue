<script setup>
defineProps({
  options: {
    type: Array,
    required: true
    // Each: { title: string, headerColor: string, pros: string[], cons: string[], recommended?: boolean }
  }
})
</script>

<template>
  <div class="pros-cons-table">
    <div
      v-for="(option, idx) in options"
      :key="idx"
      class="option-card"
      :class="{ recommended: option.recommended }"
    >
      <div class="card-header" :style="{ backgroundColor: option.headerColor }">
        {{ option.title }}
        <span v-if="option.recommended"> ★ Recommended</span>
      </div>
      <div class="card-body">
        <div class="pros-section">
          <div class="section-title pros-title">✓ PROS</div>
          <div class="section-content">
            <div v-for="(pro, pIdx) in option.pros" :key="pIdx">{{ pro }}</div>
          </div>
        </div>
        <div class="cons-section">
          <div class="section-title cons-title">✕ CONS</div>
          <div class="section-content">
            <div v-for="(con, cIdx) in option.cons" :key="cIdx">{{ con }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pros-cons-table {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.2rem;
  margin-top: 1.5rem;
}

.option-card {
  border: 1px solid #e5e7eb;
  border-radius: 10px;
  overflow: hidden;
}

.option-card.recommended {
  border: 2px solid #16a34a;
}

.card-header {
  color: white;
  padding: 0.8rem;
  text-align: center;
  font-weight: 600;
  font-size: 0.9rem;
}

.card-body {
  padding: 0.8rem;
}

.section-title {
  font-size: 0.7rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
}

.pros-title {
  color: #16a34a;
}

.cons-title {
  color: #ef4444;
}

.section-content {
  font-size: 0.7rem;
  color: #666;
  line-height: 1.6;
  margin-bottom: 0.8rem;
}

.pros-section {
  margin-bottom: 0.5rem;
}
</style>
