<template>
  <div class='type-list'>
    <button class='tab-button' :class='{ active: modelValue === "all" }' @click='set' value='all'>Все</button>
    <button class='tab-button' :class='{ active: modelValue === "jun" }' @click='set' value='jun'>Junior</button>
    <button class='tab-button' :class='{ active: modelValue === "mid" }' @click='set' value='mid'>Middle</button>
    <button class='tab-button' :class='{ active: modelValue === "sen" }' @click='set' value='sen'>Senior</button>
    <button class='tab-button' :class='{ active: modelValue === "oth" }' @click='set' value='oth'>Other</button>
  </div>
</template>

<script>
export default {
  name: 'TabList',
  props: ['modelValue'],
  methods: { set }
}

function set (el) {
  this.$emit('update:modelValue', el.target.value)
}
</script>

<style lang='stylus'>
.type-list


  .tab-button
    background #4B4
    border none
    border-top 1px solid #070
    border-bottom 1px solid #070
    color #FFF
    cursor pointer
    padding 8px 12px
    &.active
      background #FFF
      color #000
      cursor default
    &:first-child
      border-radius 5px 0 0 5px
      border-left 1px solid #070
    &:last-child
      border-radius 0 5px 5px 0
      border-right 1px solid #070
</style>