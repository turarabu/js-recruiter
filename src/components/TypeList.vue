<template>
  <div class='tab-list'>
    <button class='tab-button' :class='{ active: modelValue === "cv" }' @click='set' value='cv'>Резюме</button>
    <button class='tab-button' :class='{ active: modelValue === "va" }' @click='set' value='va'>Вакансии</button>
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
.tab-list
  margin-top 24px

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