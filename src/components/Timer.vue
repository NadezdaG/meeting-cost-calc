<template>
  <div class="timer-block">
    <div class="timer" v-html="formattedTime"></div>
    <div>
      <button @click.prevent="start" :disabled="timer !== false">Start</button>
      <button @click.prevent="stop" :disabled="timer == false">Stop</button>
    </div>
    <span class="price" v-html="total"></span>
  </div>
</template>

<script setup>
import { toRef, ref, computed } from 'vue'
const props = defineProps(['data', 'milliseconds'])
let milliseconds = toRef(props, 'milliseconds')
let data = toRef(props, 'data')
const emit = defineEmits(['updateMs'])
const timer = ref(false)

const formattedTime = computed(() => {
  const ms = milliseconds.value

  const hours = Math.floor(ms / 3600000)
  const minutes = Math.floor((ms % 3600000) / 60000)
  const seconds = Math.floor((ms % 60000) / 1000)

  return `<span>${hours}</span>:<span>${minutes}</span>:<span>${seconds}</span>:<span>${ms % 1000}</span>`
})

// computed value
const total = computed(() => {
  const ms = milliseconds.value

  const hours = ms / 3600000
  console.log('ms', ms, 'hours', hours)
  if (hours === 0) return '£0.00' // No cost if less than an hour
  var value =
    hours *
    (data.value.devs.amount * data.value.devs.price +
      data.value.pm.amount * data.value.pm.price +
      data.value.sm.amount * data.value.sm.price +
      data.value.ds.amount * data.value.ds.price)
  return '£' + value.toFixed(2)
})

// ⏱ Start the timer
const start = () => {
  if (timer.value !== false) return

  timer.value = setInterval(() => {
    emit('updateMs', milliseconds.value + 100) // tick every 100ms
  }, 100)
}

// 🛑 Stop the timer
const stop = () => {
  clearInterval(timer.value)
  timer.value = false
}
</script>

<style scoped lang="scss">
.timer-block {
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

.timer {
  font-family: monospace, 'Courier New', Courier;
  display: inline-block;
  text-align: center;
  font-variant-numeric: tabular-nums; /* helps in some modern fonts */
  * {
    display: inline-block;
    width: 2em; /* Adjust width for better alignment */
    &:last-child {
      width: 3em; /* Wider for milliseconds */
    }
  }
}
</style>
