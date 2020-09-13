<template>
  <div>
    <ReserveCalendarWeekCommon
      v-if="radios"
      :times="times"
      :first-lists="daysData"
      :second-lists="apiData.terapisLists"
      :radios="radios"
      :customer-lists="customerLists"
      :date-item="dateItem"
      :business-time="apiData.business_time"
    >
      <template #summaryItem>
        <div>
          <ReserveWeekNumberBox
            :number-of-reservations-per="apiData.numberOfReservationsPer"
          />
        </div>
      </template>
    </ReserveCalendarWeekCommon>
    <ReserveCalendarWeekCommon
      v-else
      :times="times"
      :first-lists="days"
      :second-lists="apiData.unitLists"
      :radios="radios"
      :customer-lists="customerLists"
      :date-item="dateItem"
      :business-time="apiData.business_time"
    >
      <template #summaryItem>
        <div>
          <ReserveWeekNumberBox
            :number-of-reservations-per="apiData.numberOfReservationsPer"
          />
        </div>
      </template>
    </ReserveCalendarWeekCommon>
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
      days: [],
    }
  },
  computed: {
    daysData() {
      this.setDays()
      return this.days
    },
  },
  created() {
    if (this.apiData) this.setTimes()
    this.setDays()
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
    setDays() {
      this.days = []
      const daysDate = new Date(
        this.dateItem.year + '/' + this.dateItem.month + '/' + this.dateItem.day
      )
      for (let i = 0; i < 14; i++) {
        const dayOfWeek = ['日', '月', '火', '水', '木', '金', '土'][
          daysDate.getDay()
        ]
        this.days.push({
          dayOfWeekText: daysDate.getDate() + '(' + dayOfWeek + ')',
          year: daysDate.getFullYear(),
          month: daysDate.getMonth() + 1,
          day: daysDate.getDate(),
        })
        daysDate.setDate(daysDate.getDate() + 1)
      }
    },
    radiosEvent() {
      this.$emit('sendRaiosData', 'person')
    },
    // calc
    // calc Business Time diff
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getHours() - startDate.getHours()
      return difftime
    },
    DateComparison(dateItem, date) {
      if (!dateItem) return false
      if (dateItem.year !== date.getFullYear()) return false
      if (dateItem.month !== date.getMonth() + 1) return false
      if (dateItem.day !== date.getDate()) return false
      return true
    },
    arryFindIdIndex(Id, Lists) {
      const resault = Lists.findIndex((object) => {
        return object.id === Id
      })
      return resault
    },
    calcReservationBigBoxPlaceX(Id, Lists) {
      const result = this.arryFindIdIndex(Id, Lists)
      if (result >= 0) return result
      return null
    },
    calcReservationBoxPlaceY(open, reservationDate) {
      const diffHours = Math.abs(open.getHours() - reservationDate.getHours())
      const diffMinitu =
        ((diffHours * 60 + reservationDate.getMinutes()) / 10) * 20
      return diffMinitu
    },
    calcReservationSmallBoxPlaceX(unitId, unitLists, personId, personLists) {
      const unitIdIndex = this.arryFindIdIndex(unitId, unitLists)
      const personIdIndex = this.arryFindIdIndex(personId, personLists)
      const unitPlaceX = unitIdIndex * this.apiData.terapisLists.length
      const personPlaceX = personIdIndex
      return unitPlaceX + personPlaceX
    },
  },
}
</script>
