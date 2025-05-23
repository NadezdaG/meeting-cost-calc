<script setup>
import { ref, computed } from 'vue'
const devs = ref(5)
const pm = ref(3)
const sm = ref(2)
const time = ref(0)
const seconds = ref(0)
// prices per hour
const fees = ref({
  devs: {
    name: 'Developers',
    price: 50,
  },
  pm: {
    name: 'Platform Managers / Brand people',
    price: 80,
  },
  sm: {
    name: 'Scrum Master / Project Manager',
    price: 60,
  },
})

let timer = ref(false)

// computed value
const total = computed(() => {
  var value =
    ((time.value + seconds.value / 60) / 60) *
    (devs.value * fees.value.devs.price +
      pm.value * fees.value.pm.price +
      sm.value * fees.value.sm.price)
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
  timer.value = false
}
</script>

<template>
  <h1>Meeting Cost Calculator ;)</h1>
  <div class="calc">
    <label for="devs">
      <strong>{{ fees.devs.name }}:</strong>
      <input type="number" min="0" max="10" v-model="devs" />
    </label>
    <label for="pm">
      <strong>{{ fees.pm.name }}:</strong>
      <input type="number" min="0" max="10" v-model="pm" />
    </label>
    <label for="sm">
      <strong>{{ fees.sm.name }}:</strong>
      <input type="number" min="0" max="10" v-model="sm" />
    </label>
    <label for="time">
      <strong
        >Time in minutes:<span><br />* put starting time, or leave 0</span></strong
      >
      <input type="number" min="0" max="2400" v-model="time" />
    </label>
  </div>
  <div class="total">
    <div>{{ formattedTime }}</div>
    <div>
      <button @click.prevent="start" :disabled="timer !== false">Start</button>
      <button @click.prevent="stop" :disabled="timer == false">Stop</button>
    </div>
    <span class="price" v-html="total"></span>
  </div>
  <div class="note">
    <p>
      * The hourly rates are set to average UK values in GBP. You can change them to your own
      values.
    </p>
    <label for="devs-hourly">
      <strong>{{ fees.devs.name }}</strong> Hourly Rate:
      <input type="number" id="devs-hourly" size="3" min="0" max="1000" v-model="fees.devs.price" />
    </label>
    <label for="sm-hourly">
      <strong>{{ fees.sm.name }}</strong> Hourly Rate:
      <input type="number" id="sm-hourly" size="3" min="0" max="1000" v-model="fees.sm.price" />
    </label>
    <label for="pm-hourly">
      <strong>{{ fees.pm.name }}</strong> Hourly Rate:
      <input type="number" id="pm-hourly" size="3" min="0" max="1000" v-model="fees.pm.price" />
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
    font-size: var(--font-size-sm);
    grid-template-columns: 2fr 1fr;
    gap: var(--space-xs);
    align-items: start;
  }
}
.total {
  grid-area: total;
  font-size: var(--font-size-lg);
  background-color: var(--bg-accent);
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  justify-content: center;
  align-items: center;
  .price {
    font-weight: 700;
    font-size: var(--font-size-xxl);
  }
}
.note {
  grid-area: note;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-areas: 'text text text' '. . .';
  font-size: var(--font-size-xs);
  gap: var(--space-xs);
  border-top: 1px dashed var(--border-color);
  margin-top: var(--space-sm);
  padding-top: var(--space-sm);
  label {
    display: flex;
    flex-direction: column;
  }
  p {
    grid-area: text;
  }
}
</style>
