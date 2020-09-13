<template>
  <div class="cal-top">
    <div class="nominate-mark">
      <div class="free">
        <span class="symbol"></span><span class="text">指名なし</span>
      </div>
      <div class="nominate">
        <span class="symbol"></span><span class="text">指名</span>
      </div>
    </div>
    <v-row align="center" justify="center">
      <div>
        <v-btn fab text small color="primary" @click="calendarLeftEvent">
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
      </div>
      <div>
        <v-toolbar-title>{{ dayDate }}</v-toolbar-title>
      </div>
      <div>
        <v-btn fab text small color="primary" @click="calendarRightEvent">
          <v-icon>mdi-chevron-right</v-icon>
        </v-btn>
      </div>
    </v-row>
    <v-radio-group v-if="type !== 'month'" v-model="headerRadio" row>
      <v-radio label="ユニット別" value="unit" @click="radiosEvent"></v-radio>
      <v-radio label="担当者別" value="person" @click="radiosEvent"></v-radio>
    </v-radio-group>
    <v-btn-toggle
      v-model="type"
      mandatory
      color="primary"
      @change="carendarListBtnEvent()"
    >
      <v-btn
        v-for="item in list"
        :key="item.id"
        :value="item.value"
        small
        depressed
        outlined
        >{{ item.label }}</v-btn
      >
    </v-btn-toggle>
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    radios: {
      type: String,
      default: 'unit',
    },
  },
  data() {
    return {
      headerRadio: 'unit',
      type: 'day',
      list: [
        { id: 0, label: '日', value: 'day' },
        { id: 1, label: '週', value: 'week' },
        { id: 2, label: '半月', value: 'custom-weekly' },
        { id: 3, label: '月', value: 'month' },
      ],
      weeks: ['日', '月', '火', '水', '木', '金', '土'],
      startDate: new Date(),
      dayDate: '',
    }
  },
  watch: {
    radios() {
      this.headerRadio = this.radios
    },
  },
  created() {
    this.dayDate = this.getDateToFormat(this.startDate)
    this.$emit('sendDate', this.startDate)
    this.headerRadio = this.radios
  },
  methods: {
    calendarLeftEvent() {
      if (this.type === 'day') this.addDaysToDate(-1)
      if (this.type === 'week') this.addDaysToDate(-6)
      if (this.type === 'custom-weekly') this.addDaysToDate(-13)
      if (this.type === 'month') {
        this.changeMonth(-1)
      }
      this.dayDate = this.getDateToFormat(this.startDate)
      this.$emit('sendDate', this.startDate)
    },
    calendarRightEvent() {
      if (this.type === 'day') this.addDaysToDate(1)
      if (this.type === 'week') this.addDaysToDate(6)
      if (this.type === 'custom-weekly') this.addDaysToDate(13)
      if (this.type === 'month') {
        this.changeMonth(1)
      }
      this.dayDate = this.getDateToFormat(this.startDate)
      this.$emit('sendDate', this.startDate)
    },
    getDateToFormat(startDate: Date) {
      const startDateformat =
        this.$moment(startDate).format('YYYY/MM/DD') +
        `(${this.weeks[startDate.getDay()]})`
      const endDate = new Date(startDate.valueOf())
      if (this.type === 'week') endDate.setDate(endDate.getDate() + 6)
      if (this.type === 'custom-weekly') endDate.setDate(endDate.getDate() + 13)
      const endDateformat =
        this.$moment(endDate).format('YYYY/MM/DD') +
        `(${this.weeks[endDate.getDay()]})`
      if (this.type === 'day') return startDateformat
      if (this.type === 'week') return startDateformat + ' - ' + endDateformat
      if (this.type === 'custom-weekly')
        return startDateformat + ' - ' + endDateformat
      if (this.type === 'month')
        return this.$moment(startDate).format('YYYY/MM')
      return startDateformat
    },
    addDaysToDate(noOfDays: number) {
      this.startDate.setDate(this.startDate.getDate() + noOfDays)
    },
    changeMonth(month: number) {
      this.startDate.setMonth(this.startDate.getMonth() + month)
    },
    radiosEvent() {
      this.$emit('sendRaiosData', this.headerRadio)
    },
    carendarListBtnEvent() {
      this.dayDate = this.getDateToFormat(this.startDate)
      this.$emit('sendCarendarList', this.type)
    },
  },
})
</script>
<style lang="scss" scoped>
.cal-top {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 33px;
  .next-before {
    position: relative;
    width: 230px;
    text-align: center;
    button {
      top: 0;
      bottom: 0;
      margin: auto;
      &:nth-child(1) {
        left: 0;
      }
      &:nth-child(2) {
        right: 0;
      }
    }
  }
  .nominate-mark {
    display: flex;
    & > div {
      display: flex;
      align-items: center;
      margin-right: 19px;
      .symbol {
        display: block;
        width: 66px;
        height: 20px;
        margin-right: 6px;
        border: 2px solid #6e9eff;
        border-radius: 3px;
        background: #e5eeff;
      }
      .text {
        color: #888;
      }
      &.nominate {
        .symbol {
          background: #6e9eff;
        }
      }
    }
  }
  .second-row-btn {
    box-shadow: none !important;
    width: 22px !important;
    height: 22px !important;
    border: 1px solid #c3c3c3;
  }
}
</style>
