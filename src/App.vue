<script setup>
import { ref, computed } from 'vue'
const devs = ref(5)
const pm = ref(5)
const sm = ref(5)
const time = ref(30)

// prices per hour
const fees = {
  devs: 60,
  sm: 90,
  pm: 70,
}

// computed value
const total = computed(() => {
  return (
    '£' + (time.value / 60) * (devs.value * fees.devs + pm.value * fees.pm + sm.value * fees.sm)
  )
})
</script>

<template>
  <div class="calc">
    <label for="devs">
      Developers:
      <input type="number" min="0" max="10" v-model="devs" />
    </label>
    <label for="pm">
      Platform managers:
      <input type="number" min="0" max="10" v-model="pm" />
    </label>
    <label for="sm">
      Scrum Master/Project Manager:
      <input type="number" min="0" max="10" v-model="sm" />
    </label>
    <label for="time">
      Time in minutes:
      <input type="number" min="0" max="240" v-model="time" />
    </label>
  </div>
  <div class="total">
    <strong v-html="total"></strong>
  </div>
</template>

<style scoped>
.calc {
  display: flex;
  flex-direction: column;
}

.calc label {
  display: grid;
  grid-template-columns: 2fr 1fr;
}

.total {
  font-weight: 700;
  padding-left: 50px;
  font-size: 1.5em;
}
</style>
