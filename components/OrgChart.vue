<script setup>
defineProps({
  root: {
    type: Object,
    required: true
    // { title: string, color?: string }
  },
  branches: {
    type: Array,
    required: true
    // Each: { title: string, color: string, children?: [{ label: string }] }
  }
})
</script>

<template>
  <div class="org-chart">
    <!-- Root node -->
    <div class="root-node" :style="{ backgroundColor: root.color || '#111' }">
      {{ root.title }}
    </div>
    <div class="root-connector"></div>

    <!-- Branch level -->
    <div class="branches">
      <div
        v-for="(branch, idx) in branches"
        :key="idx"
        class="branch"
      >
        <div class="branch-node" :style="{ backgroundColor: branch.color }">
          {{ branch.title }}
        </div>
        <div v-if="branch.children" class="branch-connector"></div>
        <div v-if="branch.children" class="branch-children">
          <div
            v-for="(child, cIdx) in branch.children"
            :key="cIdx"
            class="child-node"
            :style="{
              backgroundColor: `${branch.color}15`,
              borderColor: `${branch.color}60`
            }"
          >
            {{ child.label }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.org-chart {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 1.5rem;
}

.root-node {
  color: white;
  padding: 0.6rem 1.5rem;
  border-radius: 8px;
  font-size: 0.8rem;
  font-weight: 600;
}

.root-connector {
  width: 2px;
  height: 20px;
  background: #d1d5db;
}

.branches {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
}

.branch {
  text-align: center;
}

.branch-node {
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-size: 0.7rem;
  font-weight: 600;
}

.branch-connector {
  width: 2px;
  height: 15px;
  background: #d1d5db;
  margin: 0 auto;
}

.branch-children {
  display: flex;
  gap: 0.5rem;
}

.child-node {
  border: 1px solid;
  padding: 0.3rem 0.6rem;
  border-radius: 6px;
  font-size: 0.6rem;
}
</style>
