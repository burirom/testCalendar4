<template>
  <div>
    <div class="contents-block">
      <ReserveCalendarHeader
        :radios="raiosData"
        @sendDate="setDate"
        @sendRaiosData="setRaiosData"
        @sendCarendarList="setCarendarActiveList"
        @prev="prev"
        @next="next"
      />
    </div>
    <ReserveCalendarDayContent
      v-if="activeCarendarList === 'day'"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
    <ReserveCalendarWeekContent
      v-if="activeCarendarList === 'week'"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
    <ReserveCalendarHalfMonth
      v-if="activeCarendarList === 'custom-weekly'"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
    <ReserveCalendarMonthContent
      v-if="activeCarendarList === 'month'"
      ref="CalendarMonthContent"
      :date-month="date"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
  </div>
</template>
<script>
export default {
  data() {
    return {
      date: new Date(),
      dateItem: {
        year: null,
        month: null,
        day: null,
      },
      showReservationForm: false,
      raiosData: 'unit',
      activeCarendarList: 'day',
      damyData: {
        business_time: {
          open: new Date('2020/9/2/7:50'),
          close: new Date('2020/9/2/17:0'),
        },
        times: [],
        terapisLists: [
          {
            id: 1,
            terapisName: 'あ担当者1',
          },
          {
            id: 2,
            terapisName: 'い担当者2',
          },
          {
            id: 3,
            terapisName: 'う担当者３',
          },
          {
            id: 4,
            terapisName: 'え担当者４',
          },
          {
            id: 5,
            terapisName: 'お担当者5',
          },
          {
            id: 6,
            terapisName: 'お担当者5',
          },
          {
            id: 7,
            terapisName: 'お担当者5',
          },
        ],
        unitLists: [
          {
            id: 1,
            unitName: 'ユニット1',
            available: true,
          },
          {
            id: 2,
            unitName: 'ユニット2',
            available: true,
          },
          {
            id: 3,
            unitName: 'ユニット３',
            available: true,
          },
          {
            id: 4,
            unitName: 'ユニット４',
            available: false,
          },
          {
            id: 5,
            unitName: 'ユニット5',
            available: true,
          },
          {
            id: 6,
            unitName: 'ユニット４',
            available: true,
          },
          {
            id: 7,
            unitName: 'ユニット5',
            available: true,
          },
        ],
        customerLists: [
          {
            customerId: 1,
            start: new Date('2020/8/31/10:10'),
            end: new Date('2020/8/31/13:00'),
            name: {
              lastName: '紫',
              firstName: '式部',
              lastNameReading: 'ムラサキ',
              firstNameReading: 'シキブ',
            },
            customerNumberOfPeople: 0,
            nomination: false,
            terapisNameId: 6,
            unitId: 2,
            courseId: 1,
            gender: 2,
          },
          {
            customerId: 2,
            start: new Date('2020/9/2/11:00'),
            end: new Date('2020/9/2/12:00'),
            name: {
              lastName: '豊臣',
              firstName: '秀吉',
              lastNameReading: 'トヨトミ',
              firstNameReading: 'ヒデヨシ',
            },
            customerNumberOfPeople: 0,
            nomination: false,
            terapisNameId: 2,
            unitId: 3,
            courseId: 1,
            gender: 1,
          },
          {
            customerId: 3,
            start: new Date('2020/9/2/11:00'),
            end: new Date('2020/9/2/11:50'),
            name: {
              lastName: '野村',
              firstName: '耕秀',
              lastNameReading: 'ノムラ',
              firstNameReading: 'ヤスヒデ',
            },
            customerNumberOfPeople: 0,
            nomination: false,
            terapisNameId: 3,
            unitId: 3,
            courseId: 1,
            gender: 1,
          },
          {
            customerId: 4,
            start: new Date('2020/9/2/14:30'),
            end: new Date('2020/9/2/15:00'),
            name: {
              lastName: '野村',
              firstName: '耕秀',
              lastNameReading: 'ノムラ',
              firstNameReading: 'ヤスヒデ',
            },
            customerNumberOfPeople: 0,
            nomination: false,
            terapisNameId: 3,
            unitId: 3,
            courseId: 1,
            gender: 1,
          },
          {
            customerId: 5,
            start: new Date('2020/9/2/10:00'),
            end: new Date('2020/9/2/14:00'),
            name: {
              lastName: '野村',
              firstName: '耕秀',
              lastNameReading: 'ノムラ',
              firstNameReading: 'ヤスヒデ',
            },
            customerNumberOfPeople: 0,
            nomination: true,
            terapisNameId: 1,
            unitId: 2,
            courseId: 1,
            gender: 1,
          },
          {
            customerId: 6,
            start: new Date('2020/9/2/10:00'),
            end: new Date('2020/9/2/14:00'),
            name: {
              lastName: '野村',
              firstName: '耕秀',
              lastNameReading: 'ノムラ',
              firstNameReading: 'ヤスヒデ',
            },
            customerNumberOfPeople: 0,
            nomination: true,
            terapisNameId: 1,
            unitId: 3,
            courseId: 1,
            gender: 1,
          },
        ],
        courseLists: [
          {
            id: 1,
            courseName: '猫背矯正コース',
            treatmentTime: 40,
            price: 3000,
            tax: 0.1,
          },
        ],
        numberOfReservationsPer: {
          terapisLists: [
            {
              id: 1,
              terapisName: '植田',
              Number: 10,
            },
            {
              id: 2,
              terapisName: '吉田',
              Number: 20,
            },
            {
              id: 3,
              terapisName: '田中',
              Number: 30,
            },
            {
              id: 4,
              terapisName: '山田',
              Number: 40,
            },
            {
              id: 4,
              terapisName: '佐藤',
              Number: 40,
            },
            {
              id: 5,
              terapisName: '井上',
              Number: 100,
            },
            {
              id: 6,
              terapisName: '中村',
              Number: 8,
            },
          ],
          TotalNumber: 208,
          allFrame: 100,
          emptyFrame: 20,
          OccupancyRate: 38,
        },
      },
    }
  },
  methods: {
    setDate(data) {
      this.date = new Date(data)
      this.dateItem.year = data.getFullYear()
      this.dateItem.month = data.getMonth() + 1
      this.dateItem.day = data.getDate()
    },
    setRaiosData(data) {
      this.raiosData = data
    },
    setCarendarActiveList(data) {
      this.activeCarendarList = data
    },
    prev() {
      this.$refs.CalendarMonthContent.prev('prev')
    },
    next() {
      this.$refs.CalendarMonthContent.next('next')
    },
    setMonthDate(data) {
      this.monthText = data
    },
    test() {
      this.showReservationForm = true
    },
    input() {
      this.showReservationForm = false
    },
  },
}
</script>
<style lang="scss" scoped>
.contents-block {
  background: #fff;
  width: 100%;
  padding-top: 25px;
  padding-right: 25px;
  padding-left: 25px;
}
</style>
