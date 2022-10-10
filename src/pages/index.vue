<script setup lang="ts">
import * as ionicons4 from '@vicons/ionicons4'
import * as carbon from '@vicons/carbon'

const active = ref('carbon')

const list = computed(() => {
  if (active.value === 'ionicons4')
    return ionicons4
  else return carbon
})
</script>

<template>
  <div class="tabs">
    <span :class="{ active: active === 'ionicons4' }" @click="active = 'ionicons4'">ionicons4</span>
    <span :class="{ active: active === 'carbon' }" @click="active = 'carbon'">carbon</span>
  </div>
  {{ active }}
  <ul>
    <li v-for="val, key in list" :key="key">
      <component :is="val" class="icon" />
      <span>{{ key }}</span>
    </li>
  </ul>
</template>

<style>
.tabs {
  margin-bottom: 12px;
}
.tabs span {
  font-size: 16px;
  font-weight: bold;
  margin-left: 12px;
  border-bottom: 2px solid transparent;
  cursor: pointer;
}
.tabs span.active {
  border-bottom-color: black;
}
ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 12px;
  margin: 0 auto;
}

li {
  display: block;
  min-width: 180px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 8px;
}

span {
  font-size: 13px;
}

.icon {
  width: 2em;
  height: 2em;
}
</style>

<route lang="yaml">

</route>
