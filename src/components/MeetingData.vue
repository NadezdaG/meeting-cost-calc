<template>
  <section class="meeting-data">
    <label v-for="(role, key) in data" :key="key">
      <strong>{{ role.name }}:</strong>
      <input
        type="number"
        min="0"
        max="10"
        :value="role.amount"
        @input="updateAmount(key, $event.target.value)"
      />
    </label>
    <label>
      <strong>Time in minutes:<br /><span>* start time or leave 0</span></strong>
      <input type="number" min="0" max="2400" :value="minutes" @input="handleUpdateMs" />
    </label>
  </section>
</template>

<script setup>
import { toRef, computed } from 'vue'
const emit = defineEmits(['updateData', 'updateMs'])
const props = defineProps(['data', 'milliseconds'])
let milliseconds = toRef(props, 'milliseconds')

const minutes = computed(() => {
  return Math.floor(milliseconds.value / 60000) // Convert milliseconds to minutes
})

const updateAmount = (key, value) => {
  emit('updateData', key, 'amount', value)
}

const handleUpdateMs = (e) => {
  const value = e.target.value
  emit('updateMs', value * 60000) // Convert minutes to milliseconds
}
</script>

<style scoped lang="scss">
section {
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
}

label {
  display: grid;
  font-size: var(--font-size-sm);
  grid-template-columns: 2fr 1fr;
  gap: var(--space-xs);
  align-items: start;
}
</style>
