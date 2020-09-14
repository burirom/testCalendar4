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
      @openFrom="openReservationForm"
    >
      <template #ReservationForm>
        <ReservationForm
          v-if="showReservationForm"
          :names="customerLists"
          :therapists="apiData.terapisLists"
          :units="apiData.unitLists"
          :courses="apiData.courseLists"
          :value="showReservationForm"
          :date="
            new Date(dateItem.year + '/' + dateItem.month + '/' + dateItem.day)
          "
          :customer-list="sendFormCustomerData"
          @input="closeReservationForm"
        />
      </template>
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
      @openFrom="openReservationForm"
    >
      <template #ReservationForm>
        <ReservationForm
          v-if="showReservationForm"
          :names="customerLists"
          :therapists="apiData.terapisLists"
          :units="apiData.unitLists"
          :courses="apiData.courseLists"
          :value="showReservationForm"
          :date="
            new Date(dateItem.year + '/' + dateItem.month + '/' + dateItem.day)
          "
          :customer-list="sendFormCustomerData"
          @input="closeReservationForm"
        />
      </template>
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
      searchList: ['test1', 'test2'],
      showReservationForm: false,
      sendFormCustomerData: null,
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
    openReservationForm(data) {
      console.log(data)
      this.sendFormCustomerData = data
      this.showReservationForm = true
    },
    closeReservationForm() {
      this.showReservationForm = false
    },
  },
}
</script>
