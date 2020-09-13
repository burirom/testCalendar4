<template>
  <div>
    <ReserveCalendarCommon
      v-if="radios"
      :times="times"
      :first-lists="apiData.unitLists"
      :second-lists="apiData.terapisLists"
      :radios="radios"
      :customer-lists="customerLists"
      :date-item="dateItem"
      :business-time="apiData.business_time"
    >
    </ReserveCalendarCommon>
    <ReserveCalendarCommon
      v-else
      :times="times"
      :first-lists="apiData.terapisLists"
      :second-lists="apiData.unitLists"
      :radios="radios"
      :customer-lists="customerLists"
      :date-item="dateItem"
      :business-time="apiData.business_time"
    >
    </ReserveCalendarCommon>
  </div>
</template>
<script>
export default {
  props: {
    dateItem: {
      type: Object,
      default: null,
    },
    apiData: {
      type: Object,
      default: null,
    },
    customerLists: {
      type: Array,
      default: null,
    },
    radios: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      times: [],
    }
  },
  created() {
    if (this.apiData) this.setTimes()
  },
  methods: {
    setTimes() {
      this.times = []
      const businesstTime = this.calcBusinessTime(
        this.apiData.business_time.open,
        this.apiData.business_time.close
      )
      const startHour = this.apiData.business_time.open.getHours()
      for (let i = 0; i <= businesstTime; i++) {
        this.times.push(startHour + i + ':00')
      }
    },
    // calc
    // calc Business Time diff
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getHours() - startDate.getHours()
      return difftime
    },
  },
}
</script>
