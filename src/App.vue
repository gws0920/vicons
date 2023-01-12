<script setup lang="ts">
import * as ionicons4 from '@vicons/ionicons4'
import * as carbon from '@vicons/carbon'
import * as material from '@vicons/material'
import * as fluent from '@vicons/fluent'
import { ref, computed, onMounted } from 'vue'

const active = ref('carbon')
const searchVal = ref('')
const obj = computed(() => {
  if (active.value === 'ionicons4')
    return ionicons4
  else if (active.value === 'material')
    return material
  else if (active.value === 'fluent')
    return fluent
  else return carbon
})
const list = computed(() => {
  return Object.keys(obj.value).filter(item => {
    return item.toLocaleLowerCase().includes(searchVal.value.toLocaleLowerCase())
  })
})
const page = ref(1)
const virtuallyList = computed(() => {
  return list.value.slice(0, 200 * page.value)
})

const switchTab = (tab: string) => {
  active.value = tab
  page.value = 1
}

onMounted(() => {
  const el = document.getElementById('app')
  el?.addEventListener('scroll', (e) => {
    const { scrollTop, scrollHeight, clientHeight } = e.target as any
    if (virtuallyList.value.length == list.value.length) return
    if (scrollTop + clientHeight >= scrollHeight - 20) {
      page.value++
    }
  })
})

const showMsg = ref(false)
const copy = (name) => {
  navigator.clipboard.writeText(name).then(() => {
    showMsg.value = true
    setTimeout(() => {
      showMsg.value = false
    }, 5000);
  })
}
</script>

<template>
  <div class="msg" v-if="showMsg">复制成功!</div>
  <div class="header">
    <div class="tabs">
      <span :class="{ active: active === 'carbon' }" @click="switchTab('carbon')">carbon</span>
      <span :class="{ active: active === 'ionicons4' }" @click="switchTab('ionicons4')">ionicons4</span>
      <span :class="{ active: active === 'material' }" @click="switchTab('material')">material</span>
      <span :class="{ active: active === 'fluent' }" @click="switchTab('fluent')">fluent</span>
    </div>
    <div class="search">
      <input v-model="searchVal" placeholder="搜索图标" />
    </div>
  </div>
  <ul>
    <li v-for="item in virtuallyList" :key="item" @click="copy(item)">
      <component :is="obj[item]" class="icon" />
      <span>{{ item }}</span>
    </li>
  </ul>
</template>

<style>
#app {
  width: 100%;
  height: 100vh;
  overflow: auto;
}
.msg {
  position: absolute;
  top: 24px;
  right: 24px;
  border: 1px solid #ccc;
  border-radius: 4px;
  color: white;
  z-index: 1000;
  padding: 4px 12px;
  background-color: #242424;
}
@media (prefers-color-scheme: dark) {
  .header {
    background-color: #242424 !important;
  }
  .icon {
    fill: white !important;
  }
  .tabs span.active {
    border-bottom-color: white !important;
  }
}
.icon {
  fill: black;
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
  padding-bottom: 4px;
}
.tabs span.active {
  border-bottom-color: black;
}
ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 12px;
  margin: 0 auto;
  padding: 0 0 40px 0;
}

li {
  display: block;
  min-width: 200px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 8px;
}

span {
  font-size: 13px;
  cursor: pointer;
}

.icon {
  width: 2em;
  height: 2em;
}
</style>
