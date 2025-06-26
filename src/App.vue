<script setup>
import { ref, computed, reactive } from 'vue'

import MeetingData from './components/MeetingData.vue'
import Timer from './components/Timer.vue'
import FeeChange from './components/FeeChange.vue'

const milliseconds = ref(0)
// prices per hour
const data = reactive({
  devs: {
    name: 'Developers',
    price: 50,
    amount: 2,
  },
  pm: {
    name: 'Platform Managers / Brand people',
    price: 80,
    amount: 2,
  },
  sm: {
    name: 'Scrum Master / Project Manager',
    price: 60,
    amount: 1,
  },
  ds: {
    name: 'Designers',
    price: 100,
    amount: 1,
  },
})

const updateData = (line, key, val) => {
  console.log('updateData', line, key, val)
  data[line][key] = val
}

const updateMs = (value) => {
  console.log('updateMs', value)
  milliseconds.value = value
}
</script>

<template>
  <h1><img src="/flying-money.png" width="50" /> Meeting Cost Calculator ;)</h1>
  <div class="app-content">
    <MeetingData
      :data="data"
      :milliseconds="milliseconds"
      @updateMs="updateMs"
      @updateData="updateData"
    />
    <Timer :data="data" :milliseconds="milliseconds" @updateMs="updateMs" />
    <div class="note">
      <p>
        * The hourly rates are set to average UK values in GBP. You can change them to your own
        values.
      </p>
      <FeeChange :data="data" @updateData="updateData" />
    </div>
  </div>
  <footer>
    <p>Made with ❤️ by <a href="https://www.totalonion.com">Total Onion</a></p>
    <p>
      We love a good meeting - when it's actually needed. This app is a playful reminder that some
      meetings cost more than they're worth. So spend your time (and budget) wisely!
    </p>
  </footer>
</template>

<style lang="scss">
@use 'assets/scss/variables';
@use 'assets/scss/general';
#app {
}
</style>
<style scoped lang="scss">
.app-content {
  background-color: var(--bg-primary);
  padding: var(--space-lg);
  display: grid;
  gap: var(--space-md);
  grid-template-columns: 1fr;
  grid-template-areas: 'title' 'total' 'settings' 'note';
  @media (min-width: 1024px) {
    grid-template-columns: 1fr 1fr;
    grid-template-areas: 'title title' 'settings total' 'note note';
  }
}
.timer-block {
  grid-area: total;
}
h1 {
  grid-area: title;
}
.meeting-data {
  grid-area: settings;
}
.note {
  grid-area: note;
  display: grid;
  font-size: var(--font-size-xs);
  border-top: 1px dashed var(--border-color);
  margin-top: var(--space-sm);
  padding-top: var(--space-sm);
}
</style>
