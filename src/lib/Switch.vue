<template>
  <button class="wind-switch" @click="toggle" :class="{'wind-checked':value}">
    <span></span> 
  </button>
</template>

<script lang="ts">
export default {
  props: {
    value: Boolean
  },
  setup(props, context) {
    const toggle = () => {
      context.emit("update:value", !props.value)
    }
    return { 
      toggle
    }
  }
}
</script>

<style lang="scss">
$h: 22px;
$h2: $h - 4px;
.wind-switch {
  height: $h;
  width: $h*2;
  border: none;
  background: #bfbfbf;
  border-radius: $h/2;
  position: relative;
  > span {
    position: absolute; 
    top: 2px; 
    left: 2px; 
    height: $h2;   
    width: $h2;    
    background: #ffffff;     
    border-radius: $h2 / 2;   
    transition: all 250ms;
  }
  &.wind-checked { 
    background: #2269E7;
    > span {
      left: calc(100% - #{$h2} - 2px); 
    }
  }
  &:focus {
    outline: none; 
  }
  &:active {
    > span { 
      width: $h2 + 4px; 
    }
  }
  &.wind-checked:active {
    > span {
      width: $h2 + 4px;
      margin-left: -4px;
    }
  }
}
</style>