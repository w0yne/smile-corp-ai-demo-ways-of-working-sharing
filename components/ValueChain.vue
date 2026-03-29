<script setup>
defineProps({
  supportActivities: {
    type: Array,
    required: true
    // Each: { label: string, sublabel?: string, color: string }
  },
  primaryActivities: {
    type: Array,
    required: true
    // Each: { label: string, sublabel?: string, color: string }
  },
  marginLabel: { type: String, default: 'MARGIN' },
  marginColor: { type: String, default: '#16a34a' },
  marginValue: { type: String, default: '' }
})
</script>

<template>
  <div class="value-chain">
    <!-- Support activities -->
    <div class="support-row">
      <div
        v-for="(act, idx) in supportActivities"
        :key="idx"
        class="support-activity"
        :style="{ backgroundColor: act.color }"
        :class="{ 'first': idx === 0, 'last': idx === supportActivities.length - 1 }"
      >
        <div class="activity-label">{{ act.label }}</div>
        <div v-if="act.sublabel" class="activity-sublabel">{{ act.sublabel }}</div>
      </div>
    </div>

    <!-- Primary activities + margin -->
    <div class="primary-row">
      <div
        v-for="(act, idx) in primaryActivities"
        :key="idx"
        class="primary-activity"
        :style="{ backgroundColor: act.color }"
        :class="{ 'first': idx === 0 }"
      >
        <div class="activity-label" v-html="act.label.replace(/\n/g, '<br/>')"></div>
        <div v-if="act.sublabel" class="activity-sublabel" v-html="act.sublabel.replace(/\n/g, '<br/>')"></div>
      </div>
      <div class="margin-block" :style="{ backgroundColor: marginColor }">
        <div v-for="char in marginLabel" :key="char" class="margin-char">{{ char }}</div>
      </div>
    </div>

    <!-- Footer -->
    <div class="footer">
      <span>← Support Activities (top) | Primary Activities (bottom) →</span>
      <span v-if="marginValue" :style="{ color: marginColor }">Margin = {{ marginValue }}</span>
    </div>
  </div>
</template>

<style scoped>
.value-chain {
  margin: 1rem auto;
  max-width: 700px;
}

.support-row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2px;
  margin-bottom: 2px;
}

.support-activity {
  padding: 0.6rem;
  text-align: center;
}

.support-activity.first {
  border-radius: 4px 0 0 0;
}

.support-activity.last {
  border-radius: 0 4px 0 0;
}

.primary-row {
  display: grid;
  grid-template-columns: repeat(5, 1fr) 60px;
  gap: 2px;
}

.primary-activity {
  padding: 0.8rem 0.5rem;
  text-align: center;
}

.primary-activity.first {
  border-radius: 0 0 0 4px;
}

.activity-label {
  font-size: 0.7rem;
  font-weight: 600;
  color: white;
}

.activity-sublabel {
  font-size: 0.55rem;
  color: rgba(255,255,255,0.7);
  margin-top: 0.3rem;
}

.support-activity .activity-label {
  color: #7c3aed;
}

.support-activity .activity-sublabel {
  color: #888;
}

.margin-block {
  border-radius: 0 0 4px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 0.8rem 0.3rem;
}

.margin-char {
  font-size: 0.8rem;
  font-weight: 700;
  color: white;
  line-height: 1;
}

.footer {
  font-size: 0.65rem;
  color: #888;
  margin-top: 0.3rem;
  display: flex;
  justify-content: space-between;
}

.footer span:last-child {
  font-weight: 600;
}
</style>
