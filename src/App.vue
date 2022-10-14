<script setup lang="ts">
import * as ionicons4 from '@vicons/ionicons4'
import * as carbon from '@vicons/carbon'
// import * as material from '@vicons/material'
// import * as fluent from '@vicons/fluent'
import { ref, computed } from 'vue'

// TODO: 列表太长 需要优化
const active = ref('carbon')
const searchVal = ref('')
const obj = computed(() => {
  if (active.value === 'ionicons4')
    return ionicons4
  // else if (active.value === 'material')
  //   return material
  // else if (active.value === 'fluent')
  //   return material
  else return carbon
})
const list = computed(() => {
  return Object.keys(obj.value).filter(item => {
    return item.toLocaleLowerCase().includes(searchVal.value.toLocaleLowerCase())
  })
})

const switchTab = (tab: string) => {
  active.value = tab
}
</script>

<template>
  <div class="header">
    <div class="tabs">
      <span :class="{ active: active === 'carbon' }" @click="switchTab('carbon')">carbon</span>
      <span :class="{ active: active === 'ionicons4' }" @click="switchTab('ionicons4')">ionicons4</span>
      <!-- <span :class="{ active: active === 'material' }" @click="switchTab('material')">material</span>
      <span :class="{ active: active === 'fluent' }" @click="switchTab('fluent')">fluent</span> -->
    </div>
    <div class="search">
      <input v-model="searchVal" placeholder="搜索图标" />
    </div>
  </div>
  <ul>
    <li v-for="item in list" :key="item">
      <component :is="obj[item]" class="icon" />
      <span>{{ item }}</span>
    </li>
  </ul>
</template>

<style>
#app {
  width: 100%;
  min-height: 100vh;
}
@media (prefers-color-scheme: dark) {
  .header {
    background-color: #242424 !important;
  }
}
.header {
  position: sticky;
  top: 0;
  background-color: white;
  padding: 16px 0;
}
.header input {
  border-radius: 20px;
  border: 1px solid #333;
  font-size: 16px;
  padding: 8px 18px;
  transition: all .3s;
  outline: none;
  width: 400px;
}
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
  padding: 0 0 40px 0;
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
