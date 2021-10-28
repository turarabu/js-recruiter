<template>
  <div id='js-recruiter'>
    <h1 class='title'>JS Recruiter</h1>

    <TypeList v-model='type'></TypeList>

    <div class='filter-box'>
      <TabList v-model='tab'></TabList>

      <div class='right-side'>
        <select class='order' v-model='order'>
          <option value='ask'>Сначала новые</option>
          <option value='desk'>Сначала старые</option>
        </select>
        <input class='query' v-model='query' placeholder='Ключевые слова, например: vue -react -angular'>
      </div>
    </div>

    <DataTable v-bind='{ data: filtered }' @setQuery='setQuery'></DataTable>

  </div>
</template>

<script>
import TypeList from './components/TypeList.vue'
import TabList from './components/TabList.vue'
import DataTable from './components/DataTable.vue'


const vacancyList = {
  junior: require('./data/vacancy/junior.json'),
  middle: require('./data/vacancy/middle.json'),
  senior: require('./data/vacancy/senior.json'),
  others: require('./data/vacancy/others.json'),
}

const cvList = {
  junior: require('./data/cv/junior.json'),
  middle: require('./data/cv/middle.json'),
  senior: require('./data/cv/senior.json'),
  others: require('./data/cv/others.json'),
}

export default {
  name: 'JS-Recruiter',
  components: { TypeList, TabList, DataTable },
  computed: { filtered },
  methods: { setQuery },
  data: () => {
    return {
      tab: 'all',
      type: 'cv',
      order: 'ask',
      query: '',
      data: {
        va: {
          jun: vacancyList.junior,
          mid: vacancyList.middle,
          sen: vacancyList.senior,
          oth: vacancyList.others,
          all: [
            ...vacancyList.junior,
            ...vacancyList.middle,
            ...vacancyList.senior,
            ...vacancyList.others
          ]
        },

        cv: {
          jun: cvList.junior,
          mid: cvList.middle,
          sen: cvList.senior,
          oth: cvList.others,
          all: [
            ...cvList.junior,
            ...cvList.middle,
            ...cvList.senior,
            ...cvList.others
          ]
        }
      }
    }
  }
}

function filtered () {
  const result = this.data[this.type][this.tab].filter((cv) => {
    const input = cv.content.text.toLowerCase()
    const queryList = this.query.toLowerCase().split(' ')

    for ( let query of queryList ) {
      if ( query[0] === '-' ) {
        let catchQuery = query.substr(1, query.length-1)

        if ( input.search(catchQuery) >= 0 ) 
          return false
      }

      else if ( input.search(query) < 0 )
        return false
    }

    return true
  })

  return result.sort((a, b) => {
    let sortOrder = 0

    if ( a.date < b.date )
      sortOrder = 1
    
    else if ( a.date > b.date )
      sortOrder = -1

    return sortOrder === 0
      ? 0
      : this.order === 'ask'
        ? sortOrder
        : sortOrder * -1
  })
}

function setQuery (query) {
  const total = window.scrollY
  const step = total / (750 / 25)
  console.log({ query, total, step })
  const interval = setInterval(() => {
    window.scrollBy(0, -step)

    if ( Math.abs(window.scrollY) < 10 ) {
      clearInterval(interval)
      this.query = query
    }
  }, 25)
}
</script>

<style lang='stylus'>
*
  box-sizing border-box
  margin 0
  padding 0
  transition all .15s

#js-recruiter
  font-family 'Open Sans', sans-serif
  margin 18px auto
  width 960px

  .filter-box
    align-items center
    display flex
    justify-content space-between
    margin 12px 0 4px 0

    .order
      border-radius 5px
      margin 0 4px
      padding 4px 0
      height 34px
      width 150px

    .query
      border 1px solid #333
      border-radius 5px
      padding 4px 8px
      height 34px
      width 320px
</style>
