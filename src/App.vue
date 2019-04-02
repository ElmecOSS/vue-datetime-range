<template>
  <div id="app">
    <DateTimeRange v-model = "val"></DateTimeRange>
  </div>
</template>

<script>
import DateTimeRange from './components/DateTimeRange'

export default {
  name: 'App',
  components: {
    DateTimeRange
  },
  data () {
    return {
      val: {start: new Date().toString(), end: (new Date(Date.now() - (1 * 24 * 3600 * 1000)).toString())}
    }
  }
}
</script>

<style>
</style>
