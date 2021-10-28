<template>
  <div class='data-tab'>
    <div class='cv' v-for='cv of parse' :key='cv.id'>
      <div class='cv-info'>
        <div class='avatar-box'>
          <div class='avatar'></div>
        </div>

        <span class='author'>{{ cv.user_name }}</span>
        <span class='date-key'>Дата заявки:</span>
        <span class='date-value'>{{ getDate(cv.date) }}</span>
        <span class='date-value'>{{ getTime(cv.date) }}</span>
      </div>

      <p class='cv-content' v-html='cv.content.html'></p>
    </div>

    <div class='page-control'>
      <button class='button' v-bind='{ disabled: page === 1 }' @click='go(-1)'>← Туда</button>
      <p class='page-info'>Результаты {{ (( page - 1) * rows) + 1 }}-{{ Math.min(data.length, (page * rows)) }}
        из {{ data.length }}</p>
      <button class='button' v-bind='{ disabled: page === Math.ceil(data.length / rows) }' @click='go(+1)'>Сюда →</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TabList',
  props: ['data'],
  computed: { parse },
  methods: { go, getDate, getTime },
  mounted: mounted,
  data: () => {
    return {
      page: 1,
      rows: 30
    }
  }
}

function parse () {
  return this.data.slice((this.page - 1) * this.rows, this.page * this.rows)
}

function go (n) {
  const min = 1
  const max = Math.ceil(this.data.length / this.rows)

  this.page = Math.max(min, Math.min(max, this.page + n))
}

function getDate (string) {
  const date = new Date(string)

  return date.toLocaleString('ru', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

function getTime (string) {
  return new Date(string).toLocaleString('ru', {
    hour: 'numeric',
    minute: 'numeric',
    second: 'numeric'
  })
}

// mounted
function mounted () {
  this.$watch('data', () => {
    this.page = 1
  })

  window.addEventListener('click', (event) => {
    for ( let el of event.path ) {
      if ( el.classList.contains('hashtag') === true )
        return this.$emit('set-query', el.innerText)
    }
  })
}
</script>

<style lang='stylus'>
.data-tab

  .cv
    border-radius 5px
    box-shadow  0 0 3px RGBA(0, 0, 0, .5)
    display flex
    margin 16px 0
    overflow hidden

  .cv-info
    align-items center
    background #DDD
    display flex
    padding 32px 0
    flex-direction column
    width 200px

    .avatar-box
      background #000
      border-radius 50%
      height 100px
      width 100px

    .author
      margin 12px 0

  .cv-content
    padding 24px 32px 24px 24px
    width 760px

    .hashtag, .link
      color RGB(80, 80, 240)
      cursor pointer
      &:hover
        text-decoration underline

    .underline
      text-decoration underline

    .strikethrough
      text-decoration line-through

  .page-control
    align-items center
    display flex
    justify-content center
    padding 8px 0

    .button
      background none
      border none
      color #33A
      cursor pointer
      font-size 18px
      margin 0 16px
      outline none
      &:disabled
        color #BBB
</style>