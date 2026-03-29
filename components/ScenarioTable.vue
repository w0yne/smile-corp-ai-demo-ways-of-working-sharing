<script setup>
defineProps({
  columns: {
    type: Array,
    required: true
    // Each: { title: string, subtitle?: string, recommended?: boolean, color?: string }
  },
  rows: {
    type: Array,
    required: true
    // Each: { label: string, values: [{ text: string, highlight?: boolean, color?: string }] }
  },
  footer: {
    type: Object,
    default: null
    // { text: string }
  }
})
</script>

<template>
  <div class="scenario-table">
    <table>
      <thead>
        <tr>
          <th></th>
          <th
            v-for="(col, idx) in columns"
            :key="idx"
            :class="{ recommended: col.recommended }"
            :style="col.recommended ? { color: col.color || '#16a34a', border: `2px solid ${col.color || '#16a34a'}` } : {}"
          >
            {{ col.title }}
            <br v-if="col.subtitle" />
            <span class="col-subtitle">{{ col.subtitle }}</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rIdx) in rows" :key="rIdx">
          <td class="row-label">{{ row.label }}</td>
          <td
            v-for="(val, vIdx) in row.values"
            :key="vIdx"
            :class="{ highlight: val.highlight }"
            :style="val.highlight ? { backgroundColor: '#f0fdf4' } : {}"
          >
            <span :style="val.color ? { color: val.color, fontWeight: 600 } : {}">{{ val.text }}</span>
          </td>
        </tr>
      </tbody>
    </table>

    <div v-if="footer" class="footer">
      <span>{{ footer.text }}</span>
    </div>
  </div>
</template>

<style scoped>
.scenario-table {
  margin: 1rem auto;
  max-width: 700px;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.75rem;
}

thead th {
  text-align: center;
  padding: 0.6rem;
  font-weight: 600;
  border-bottom: 2px solid #111;
}

thead th:first-child {
  text-align: left;
}

thead th.recommended {
  border-radius: 4px;
}

.col-subtitle {
  font-size: 0.65rem;
  font-weight: 400;
}

tbody td {
  text-align: center;
  padding: 0.6rem;
  border-bottom: 1px solid #e5e7eb;
}

.row-label {
  text-align: left;
  font-weight: 500;
}

.highlight {
  font-weight: 500;
}

.footer {
  text-align: center;
  margin-top: 0.8rem;
  padding: 0.6rem;
  background: #f0fdf4;
  border-radius: 8px;
  border: 1px solid #bbf7d0;
}

.footer span {
  font-size: 0.8rem;
  color: #16a34a;
  font-weight: 500;
}
</style>
