<script setup>
import { ref, computed } from 'vue'
const devs = ref(5)
const pm = ref(3)
const sm = ref(2)
const time = ref(0)
const seconds = ref(0)
// prices per hour
const fees = ref({
  devs: 60,
  sm: 80,
  pm: 60,
})

let timer = ref(false)

// computed value
const total = computed(() => {
  var value =
    ((time.value + seconds.value / 60) / 60) *
    (devs.value * fees.value.devs + pm.value * fees.value.pm + sm.value * fees.value.sm)
  return '£' + value.toFixed(2)
})

const formattedTime = computed(() => {
  const pad = (n) => String(n).padStart(2, '0')
  return `${pad(time.value)}:${pad(seconds.value)}`
})

const start = () => {
  // increase time value by 1 every minute
  timer.value = setInterval(() => {
    seconds.value++
    if (seconds.value >= 60) {
      time.value++
      seconds.value = 0
    }
  }, 1000)
}

const stop = () => {
  clearInterval(timer.value)
}
</script>

<template>
  <h1>Meeting Cost Calculator ;)</h1>
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
    <div>{{ formattedTime }}</div>
    <div>
      <button @click.prevent="start" :disabled="timer !== false">Start</button>
      <button @click.prevent="stop" :disabled="timer !== true">Stop</button>
    </div>
    <strong v-html="total"></strong>
  </div>
  <div class="note">
    <p>
      * The hourly rates are set to average UK values in GBP. You can change them to your own
      values.
    </p>
    <label for="devs-hourly">
      Developers Hourly Rate
      <input type="number" id="devs-hourly" size="3" min="0" max="1000" v-model="fees.devs" />
    </label>
    <label for="pm-hourly">
      Platform managers Hourly Rate:
      <input type="number" id="pm-hourly" size="3" min="0" max="1000" v-model="fees.sm" />
    </label>
    <label for="sm-hourly">
      Scrum Master/Project Manager Hourly Rate:
      <input type="number" id="sm-hourly" size="3" min="0" max="1000" v-model="fees.pm" />
    </label>
  </div>
</template>

<style lang="scss">
@use 'assets/scss/variables';
@use 'assets/scss/general';
#app {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-areas: 'title' 'total' 'settings' 'note';
  @media (min-width: 1024px) {
    grid-template-columns: 1fr 1fr;
    grid-template-areas: 'title title' 'settings total' 'note note';
  }
}
</style>
<style scoped lang="scss">
h1 {
  grid-area: title;
}
.calc {
  grid-area: settings;
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  label {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: var(--space-xs);
    align-items: start;
  }
}
.total {
  grid-area: total;
  font-weight: 700;
  font-size: var(--font-size-lg);
  background-color: var(--bg-accent);
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  justify-content: center;
  align-items: center;
}
.note {
  grid-area: note;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-areas: 'text text text' '. . .';
  p {
    grid-area: text;
  }
}
</style>
