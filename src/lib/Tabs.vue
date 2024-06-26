<template>
  <div class="wind-tabs">
    <div class="wind-tabs-nav" ref="container">
      <div class="wind-tabs-nav-item" v-for="(t,index) in titles" :ref="el => { if (t===selected) selectedItem = el }" @click="select(t)" :class="{selected: t=== selected}" :key="index">{{t}}</div>
      <div class="wind-tabs-nav-indicator"  ref="indicator"></div>
    </div>
    <div class="wind-tabs-content">
      <component :is="current" :key="current.props.title" />
    </div>
  </div>
</template>

<script lang="ts">
import Tab from './Tab.vue';
import { computed, ref, watchEffect, onMounted } from 'vue';

export default {
  props: {
    selected: {
      type: String
    }
  },
  setup(props, context) {
    const selectedItem = ref < HTMLDivElement > (null)
    const indicator = ref < HTMLDivElement > (null)
    const container = ref < HTMLDivElement > (null)
    onMounted(() => {
      watchEffect(() => {
        const { width } = selectedItem.value.getBoundingClientRect()
        indicator.value.style.width = width + 'px'
        const { left: left1 } = container.value.getBoundingClientRect()
        const { left: left2 } = selectedItem.value.getBoundingClientRect()
        const left = left2 - left1
        indicator.value.style.left = left + 'px'
      })
    })

    const defaults = context.slots.default()
    defaults.forEach((tag) => {
      // @ts-ignore
      if (tag.type.name !== Tab.name) {
        throw new Error('Tabs 子标签必须是 Tab')
      }
    })

    const current = computed(() => {
      return defaults.find(tag => tag.props.title === props.selected)
    })
    
    const titles = defaults.map((tag) => {
      return tag.props.title
    })

    const select = (title: string) => {
      context.emit('update:selected', title)
    }

    return {
      current, defaults, titles, select, selectedItem, indicator, container
    }
  }
}
</script>

<style lang="scss">
.wind-tabs {
  &-nav {
    display: flex;
    color: #44475b;
    border-bottom: 1px solid #d9d9d9;
    position: relative;
    &-item {
      padding: 8px 0;
      margin: 0 16px;
      cursor: pointer;
      &:first-child {
        margin-left: 0;
      }
      &.selected {
        color: #2269E7;
      }
    }
    &-indicator {
      position: absolute;
      height: 3px;
      background: #2269E7;
      left: 0;
      bottom: -1px;
      width: 100px;
      transition: all 250ms;
    }
  }
  &-content {
    padding: 8px 0;
  }
}
</style>