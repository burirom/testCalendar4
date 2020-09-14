```
<template>
  <div>
    <ReserveCalendarWeekCommon
      v-if="radios"
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
          <div class="d-flex">
            <div>ユニット別</div>
            <v-icon v-if="activeSelectList" @click="closeList"
              >mdi-chevron-down</v-icon
            >
            <v-icon v-else @click="openList">mdi-chevron-up</v-icon>
          </div>
          <div v-if="activeSelectList">
            <div v-for="(unitList, index) in apiData.unitLists" :key="index">
              <v-checkbox
                v-model="selectUnitLists[index]"
                :label="unitList.unitName"
                color="primary"
                value="primary"
                hide-details
              ></v-checkbox>
            </div>
          </div>
        </div>
      </template>
    </ReserveCalendarWeekCommon>
    <ReserveCalendarWeekCommon
      v-else
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
          <div class="d-flex">
            <div>担当者</div>
            <v-icon v-if="activeSelectList" @click="closeList"
              >mdi-chevron-down</v-icon
            >
            <v-icon v-else @click="openList">mdi-chevron-up</v-icon>
          </div>
          <div v-if="activeSelectList">
            <div
              v-for="(terapistList, index) in apiData.terapisLists"
              :key="index"
            >
              <v-checkbox
                v-model="selectTerapisLists[index]"
                :label="terapistList.terapisName"
                color="primary"
                value="primary"
                hide-details
              ></v-checkbox>
            </div>
          </div>
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
      searchList: ['test1', 'test2'],
      selectUnitLists: [],
      selectTerapisLists: [],
      activeSelectList: true,
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
    this.setSelectLists()
  },
  methods: {
    openList() {
      this.activeSelectList = true
    },
    closeList() {
      this.activeSelectList = false
    },
    setSelectLists() {
      for (let i = 0; i < this.apiData.unitLists.length; i++) {
        this.selectUnitLists.push({
          active: true,
        })
      }
      for (let i = 0; i < this.apiData.terapisLists.length; i++) {
        this.selectTerapisLists.push({
          active: true,
        })
      }
    },
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
      for (let i = 0; i < 7; i++) {
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
    // calc
    // calc Business Time diff
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getHours() - startDate.getHours()
      return difftime
    },
  },
}
</script>
```
