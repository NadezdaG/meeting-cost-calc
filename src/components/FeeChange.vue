<template>
  <section>
    <label for="devs-hourly">
      <strong>{{ data.devs.name }}</strong> Hourly Rate:
      <input
        type="number"
        id="devs-hourly"
        size="3"
        min="0"
        max="1000"
        :value="data.devs.price"
        @input="updateFee('devs', e.target.value)"
      />
    </label>
    <label for="sm-hourly">
      <strong>{{ data.sm.name }}</strong> Hourly Rate:
      <input
        type="number"
        id="sm-hourly"
        size="3"
        min="0"
        max="1000"
        :value="data.sm.price"
        @input="updateFee('sm', e.target.value)"
      />
    </label>
    <label for="pm-hourly">
      <strong>{{ data.pm.name }}</strong> Hourly Rate:
      <input
        type="number"
        id="pm-hourly"
        size="3"
        min="0"
        max="1000"
        :value="data.pm.price"
        @input="updateFee('pm', e.target.value)"
      />
    </label>
    <label for="ds-hourly">
      <strong>{{ data.ds.name }}</strong> Hourly Rate:
      <input
        type="number"
        id="ds-hourly"
        size="3"
        min="0"
        max="1000"
        :value="data.ds.price"
        @input="updateFee('ds', e.target.value)"
      />
    </label>
  </section>
</template>

<script setup>
import { toRef, ref, computed } from 'vue'
const props = defineProps(['data'])
let data = toRef(props, 'data')
const emit = defineEmits(['updateData'])

const updateFee = (key, value) => {
  emit('updateData', key, 'price', value)
}
</script>

<style scoped lang="scss">
section {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: var(--space-xs);
}
label {
  display: flex;
  flex-direction: column;
}
</style>
