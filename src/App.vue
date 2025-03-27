<script setup>
import { ref, computed } from 'vue'
const devs = ref(5)
const pm = ref(3)
const sm = ref(2)
const time = ref(0)
const seconds = ref(0)
// prices per hour
const fees = {
  devs: 60,
  sm: 90,
  pm: 70,
}

let timer = false

// computed value
const total = computed(() => {
  var value =
    ((time.value + seconds.value / 60) / 60) *
    (devs.value * fees.devs + pm.value * fees.pm + sm.value * fees.sm)
  return '£' + value.toFixed(2)
})

const start = () => {
  // increase time value by 1 every minute
  timer = setInterval(() => {
    seconds.value++
    if (seconds.value >= 60) {
      time.value++
      seconds.value = 0
    }
  }, 1000)
}

const stop = () => {
  clearInterval(timer)
}
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
      <input type="number" min="0" max="2400" v-model="time" />
    </label>
    * put time spent, or start a timer.
  </div>
  <div class="total">
    <div><span v-html="time"></span>:<span v-html="seconds"></span></div>
    <div>
      <button @click.prevent="start">Start</button>
      <button @click.prevent="stop">Stop</button>
    </div>
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
