<script setup>
import { ref, computed, reactive } from 'vue'

import MeetingData from './components/MeetingData.vue'
import Timer from './components/Timer.vue'

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
  <h1>Meeting Cost Calculator ;)</h1>
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
    <label for="devs-hourly">
      <strong>{{ data.devs.name }}</strong> Hourly Rate:
      <input type="number" id="devs-hourly" size="3" min="0" max="1000" v-model="data.devs.price" />
    </label>
    <label for="sm-hourly">
      <strong>{{ data.sm.name }}</strong> Hourly Rate:
      <input type="number" id="sm-hourly" size="3" min="0" max="1000" v-model="data.sm.price" />
    </label>
    <label for="pm-hourly">
      <strong>{{ data.pm.name }}</strong> Hourly Rate:
      <input type="number" id="pm-hourly" size="3" min="0" max="1000" v-model="data.pm.price" />
    </label>
    <label for="ds-hourly">
      <strong>{{ data.ds.name }}</strong> Hourly Rate:
      <input type="number" id="ds-hourly" size="3" min="0" max="1000" v-model="data.ds.price" />
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
.note {
  grid-area: note;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-areas: 'text text text text' '. . . .';
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
