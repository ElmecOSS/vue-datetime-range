<template>
  <div class="row">
      <div class="col-md-6">
        <div class='page-header'>
          <h4> Absolute Time</h4>
        </div>
        <div class="row form">
          <div class="col-md-6 center-box">
            <label for="start_date">Start Date</label>
            <div class="form-group">
              <div class="col-sm-12">
                <date-picker :not-after="end_value" placeholder="Starting date" id="start_date" v-model="start_value" :time-picker-options="timePickerOptions" lang="it" type="datetime" format="YYYY-MM-DD HH:mm:ss"></date-picker>
              </div>
            </div>
          </div>
          <div class="col-md-6 center-box">
            <div class="form-group">
              <label for="end_date">End Date</label>
              <div class="col-sm-12">
                <date-picker :not-before="start_value"  placeholder="End date" id="end_date" v-model="end_value" :time-picker-options="timePickerOptions"  lang="it" type="datetime" format="YYYY-MM-DD HH:mm:ss"></date-picker>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class='page-header'>
          <h4>Quick</h4>
        </div>
        <div class="row">
          <div class="col-md-6" :key="index" v-for="(data,index) in chunkArray(relativeDate.length/2)">
            <ul class="list-group list-group-flush">
              <li :key="index" v-for="(elem, index) in data" class="list-group-item list-group-item-action" :class="{ active: isHintActive(elem.value) }" @click="calculateInterval(elem.value)">{{elem.label}}</li>
            </ul>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
export default {
  components: { DatePicker },
  name: 'date-time-range',
  props: {
    value: {
      type: Object
    },
    relativeDate: {
      type: Array,
      default: function () {
        return [
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
  },
  data () {
    return {
      start_value: '',
      end_value: '',
      shortcuts: [
        {
          text: 'Today'
        }
      ],
      timePickerOptions: {
        start: '00:00',
        step: '00:30',
        end: '23:30'
      }
    }
  },
  mounted: function () {
    this.setInitValue()
  },
  methods: {
    calculateInterval: function (interval) {
      if (interval < 0) {
        let end = Date.now()
        this.start_value = new Date((end + interval)).toString()
        this.end_value = new Date(end).toString()
      } else {
        let start = Date.now()
        this.start_value = new Date(start).toString()
        this.end_value = new Date(start + interval).toString()
      }
    },
    setInitValue: function () {
      if (this.value === undefined || this.value['start'] == null || this.value['start'] === undefined || this.value['end'] == null || this.value['end'] === undefined) {
        this.start_value = Date.now()
        this.end_value = Date.now()
      }
      this.start_value = this.value['start']
      this.end_value = this.value['end']
    },
    isHintActive: function (hintSecond) {
      let dif = new Date(this.start_value).getTime() - new Date(this.end_value).getTime()
      return Math.abs(dif) === Math.abs(hintSecond)
    },
    chunkArray: function (chunkSize) {
      let index = 0
      let arrayLength = this.relativeDate.length
      let tempArray = []
      for (index = 0; index < arrayLength; index += chunkSize) {
        let myChunk = this.relativeDate.slice(index, index + chunkSize)
        // Do something if you want with the group
        tempArray.push(myChunk)
      }
      return tempArray
    }
  },
  watch: {
    value: function () {
      this.setInitValue()
    },
    start_value: function () {
      this.$emit('input', {start: this.start_value, end: this.end_value})
    },
    end_value: function () {
      this.$emit('input', {start: this.start_value, end: this.end_value})
    }
  }
}
</script>

<style scoped>
  .page-header {
    text-align: center;
  }
  .center-box {
    text-align: center;
  }
</style>
