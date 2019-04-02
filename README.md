# Vue-Datetime-Range

> Vue.js datetime range

### Example Usage
```html
<template>
  <div id="app">
    <DateTimeRange :relativeDate="relData" v-model = "val"></DateTimeRange>
  </div>
</template>

<script>
import DateTimeRange from 'vue-datetime-range'

export default {
  components: {
    DateTimeRange
  },
  data () {
    return {
      val: {start: new Date().toString(), end: (new Date(Date.now() - (1 * 24 * 3600 * 1000)).toString())}
      relData: [
         {
           label: 'Last 1 hours',
           value: -3600000
         },
         {
           label: 'Last 3 hours',
           value: -10800000
         },
         {
           label: 'Last 6 hours',
           value: -21600000
         },
         {
           label: 'Last 12 hours',
           value: -43200000
         },
         {
           label: 'Last 24 hours',
           value: -86400000
         },
         {
           label: 'Last 30 days',
           value: -2592000000
         },
         {
           label: 'Last 60 days',
           value: -5184000000
         },
         {
           label: 'Last 90 days',
           value: -7776000000
         },
         {
           label: 'Last 6 months',
           value: -15552000000
         },
         {
           label: 'Last 1 years',
           value: -31104000000
         }
       ]
    }
  }
}
</script>
```


### License

The code is available under the [GNU GPL v3.0](LICENSE) license.
