<script setup>
defineProps({
  rootQuestion: { type: String, required: true },
  branches: {
    type: Array,
    required: true
    // Two main branches: [leftBranch, rightBranch]
    // Each branch: { answer: string, answerColor: string, question?: string, outcomes?: [{ answer: string, answerColor: string, label: string, color: string, bgColor: string }] }
  }
})
</script>

<template>
  <div class="decision-tree">
    <!-- Root question -->
    <div class="root-node">{{ rootQuestion }}</div>

    <!-- First level branches -->
    <div class="branches-level-1">
      <div
        v-for="(branch, idx) in branches"
        :key="idx"
        class="branch"
      >
        <div class="answer-label" :style="{ color: branch.answerColor }">{{ branch.answer }}</div>
        <div class="connector" :style="{ backgroundColor: branch.answerColor }"></div>

        <div v-if="branch.question" class="question-node">{{ branch.question }}</div>

        <!-- Second level outcomes -->
        <div v-if="branch.outcomes" class="outcomes">
          <div
            v-for="(outcome, oIdx) in branch.outcomes"
            :key="oIdx"
            class="outcome"
          >
            <div class="outcome-answer" :style="{ color: outcome.answerColor }">{{ outcome.answer }}</div>
            <div class="outcome-connector" :style="{ backgroundColor: outcome.answerColor }"></div>
            <div
              class="outcome-node"
              :style="{ backgroundColor: outcome.bgColor, borderColor: outcome.color, color: outcome.color }"
            >
              <span v-html="outcome.label.replace(/\n/g, '<br/>')"></span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.decision-tree {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 1.5rem;
}

.root-node {
  background: #111;
  color: white;
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  font-size: 0.8rem;
  font-weight: 600;
}

.branches-level-1 {
  display: flex;
  gap: 8rem;
  margin-top: 0.5rem;
}

.branch {
  text-align: center;
}

.answer-label {
  font-size: 0.7rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
}

.connector {
  width: 2px;
  height: 20px;
  margin: 0 auto;
}

.question-node {
  background: #f8fafc;
  border: 1px solid #e5e7eb;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-size: 0.75rem;
  font-weight: 500;
  margin-top: 0.3rem;
}

.outcomes {
  display: flex;
  gap: 3rem;
  margin-top: 0.4rem;
}

.outcome {
  text-align: center;
}

.outcome-answer {
  font-size: 0.65rem;
}

.outcome-connector {
  width: 1px;
  height: 15px;
  margin: 0 auto;
}

.outcome-node {
  padding: 0.4rem 0.8rem;
  border-radius: 8px;
  border: 2px solid;
  font-size: 0.7rem;
  font-weight: 600;
  margin-top: 0.2rem;
}
</style>
