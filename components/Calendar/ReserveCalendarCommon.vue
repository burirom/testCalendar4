<template>
  <div class="box" style="width: 100%">
    <!-- Reservation Form -->
    <slot name="ReservationForm"></slot>
    <!-- header -->
    <div class="d-flex header">
      <div class="first-content" style="min-width: 200px; max-width: 200px">
        <div class="second-item first-row"><slot name="first" /></div>
        <div class="second-item second-row"></div>
      </div>
      <div
        v-for="(firstList, firstIndex) in firstLists"
        :key="firstIndex"
        class="second-content col-Width"
        :style="
          cheackActiveSecondList(firstIndex) ? colWidth() : defaultColWidth()
        "
      >
        <div v-if="radios">
          <div v-if="!firstList.available" class="available-unit"></div>
        </div>
        <div
          v-if="firstList.unitName"
          class="second-item first-row d-flex align-center justify-center"
        >
          {{ firstList.unitName }}
        </div>
        <div
          v-else-if="firstList.terapisName"
          class="second-item first-row d-flex align-center justify-center"
        >
          {{ firstList.terapisName }}
        </div>
        <div v-else class="second-item first-row">
          {{ firstList }}
        </div>
        <!-- close secondList -->
        <div
          v-if="!cheackActiveSecondList(firstIndex)"
          class="second-item second-row icon-box second-row-text"
        >
          <div
            v-if="radios"
            class="first-row d-flex align-center justify-center"
          >
            担当者表示
          </div>
          <div v-else class="first-row d-flex align-center justify-center">
            ユニット表示
          </div>
          <v-icon class="icon-btn" @click="openSecondList(firstIndex)">
            mdi-chevron-right
          </v-icon>
        </div>
        <!-- open secondList -->
        <div
          v-else
          class="d-flex icon-box second-item second-row second-row-text"
        >
          <div
            v-for="(secondList, secondListIndex) in secondLists"
            :key="secondListIndex"
            class="border-right second-col-Width d-flex align-center justify-center"
          >
            <div v-if="radios">
              {{ secondList.terapisName.substring(0, 1) }}
            </div>
            <div v-else>
              {{ secondList.unitName.substring(0, 1) }}
            </div>
          </div>
          <v-icon class="icon-btn" @click="closeSecondList(firstIndex)">
            mdi-chevron-left
          </v-icon>
        </div>
      </div>
    </div>
    <!-- body -->
    <div class="d-flex body">
      <!-- time col -->
      <div class="time-content" style="min-width: 200px">
        <div
          v-for="(time, timeIndex) in times"
          :key="timeIndex"
          class="time-row"
        >
          <slot name="summaryItem" />
          <p class="calender-time-table ma-0 pl-2 pr-2">{{ time }}</p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        </div>
      </div>
      <!-- table -->
      <div class="d-flex posion-rl">
        <!-- table row -->
        <div
          v-for="(firstList, firstIndex) in firstLists"
          :key="firstIndex"
          class="table-col col-Width"
          :style="
            cheackActiveSecondList(firstIndex) ? colWidth() : defaultColWidth()
          "
        >
          <!-- table col -->
          <!-- reservationbox -->
          <div v-if="cheackActiveSecondList(firstIndex)">
            <div
              v-for="(customerList, index) in customerLists"
              :key="`first-${index}`"
              class="item-handle"
            >
              <ReservationBox
                v-if="
                  cheackIndexId(customerList, firstLists, firstIndex) &&
                  DateComparison(dateItem, customerList.start)
                "
                :box-shadow="boxShadow(customerList)"
                :row-width="40"
                :customer-info="customerList"
                :box-place-x="
                  calcReservationSmallBoxPlaceX(customerList, firstLists)
                "
                :box-place-y="
                  calcReservationBoxPlaceY(
                    businessTime.open,
                    customerList.start
                  )
                "
                @boxClick="openReservationForm(customerList)"
              />
            </div>
          </div>
          <div v-if="!cheackActiveSecondList(firstIndex)">
            <div
              v-for="(customerList, index) in customerLists"
              :key="`first-${index}`"
              class="item-handle"
            >
              <ReservationBox
                v-if="
                  cheackIndexId(customerList, firstLists, firstIndex) &&
                  DateComparison(dateItem, customerList.start)
                "
                :box-shadow="boxShadow(customerList)"
                :row-width="100"
                :customer-info="customerList"
                :box-place-x="0"
                :box-place-y="
                  calcReservationBoxPlaceY(
                    businessTime.open,
                    customerList.start
                  )
                "
                @boxClick="openReservationForm(customerList)"
              />
            </div>
          </div>
          <div
            v-for="(time, timeIndex) in times"
            :key="timeIndex"
            class="d-flex table-row"
            @click="openReservationForm(null)"
          >
            <!-- close secondList -->
            <div
              v-if="!cheackActiveSecondList(firstIndex)"
              class="table-row"
              style="width: 100%"
            >
              <!-- unit  available box -->
              <div v-if="radios">
                <div
                  v-if="!firstList.available"
                  class="available-unit"
                  style="width: 100%"
                ></div>
              </div>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            </div>
            <!-- open secondList -->
            <div v-else class="d-flex table-row">
              <!-- unit  available box -->
              <div v-if="radios">
                <div
                  v-if="!firstList.available"
                  class="available-unit"
                  style="width: 100%"
                ></div>
              </div>
              <div
                v-for="(secondList, secondListIndex) in secondLists"
                :key="secondListIndex"
                class="border-right second-col-Width"
              >
                <div v-if="!radios">
                  <div
                    v-if="!secondList.available"
                    class="available-unit"
                    style="width: 40px"
                  ></div>
                </div>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
                <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    firstLists: {
      type: Array,
      default: null,
    },
    secondLists: {
      type: Array,
      default: null,
    },
    times: {
      type: Array,
      default: null,
    },
    width: {
      type: Number,
      default: 150,
    },
    radios: {
      type: Boolean,
      default: true,
    },
    customerLists: {
      type: Array,
      default: null,
    },
    dateItem: {
      type: Object,
      default: null,
    },
    businessTime: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      activeNumber: -1,
      openSecondListIndex: [],
      customCustomerLists: [],
      showReservationForm: false,
    }
  },
  computed: {
    colWidth() {
      return function () {
        return { '---width': this.secondLists.length * 40 + 'px' }
      }
    },
    defaultColWidth() {
      return function () {
        return { '---width': 100 + 'px' }
      }
    },
    cheackActiveSecondList() {
      return function (index) {
        const result = this.openSecondListIndex.some(function (value) {
          return value === index
        })
        return result
      }
    },
  },
  watch: {
    radios() {
      this.openSecondListIndex = []
    },
  },
  created() {
    this.openSecondListIndex = []
  },
  methods: {
    boxShadow(customerList) {
      const resault = []
      if (this.radios) {
        this.customerLists.forEach((list) => {
          if (list.customerId === customerList.customerId) return false
          if (list.unitId !== customerList.unitId) return false
          if (list.start.getFullYear() !== customerList.start.getFullYear())
            return false
          if (list.start.getMonth() + 1 !== customerList.start.getMonth() + 1)
            return false
          if (list.start.getDate() !== customerList.start.getDate())
            return false
          if (list.start < customerList.end && list.end > customerList.start) {
            resault.push(true)
          }
        })
      } else {
        this.customerLists.forEach((list) => {
          if (list.customerId === customerList.customerId) return false
          if (list.unitId !== customerList.unitId) return false
          if (list.terapisNameId !== customerList.terapisNameId) return false
          if (list.start.getFullYear() !== customerList.start.getFullYear())
            return false
          if (list.start.getMonth() + 1 !== customerList.start.getMonth() + 1)
            return false
          if (list.start.getDate() !== customerList.start.getDate())
            return false
          if (list.start < customerList.end && list.end > customerList.start) {
            resault.push(true)
          }
        })
      }
      if (resault.length > 0) return true
      return false
    },
    openSecondList(index) {
      this.openSecondListIndex.push(index)
    },
    closeSecondList(firstListIndex) {
      const index = this.openSecondListIndex.findIndex(
        (v) => v === firstListIndex
      )
      this.openSecondListIndex.splice(index, 1)
    },
    DateComparison(dateItem, date) {
      if (!dateItem) return false
      if (dateItem.year !== date.getFullYear()) return false
      if (dateItem.month !== date.getMonth() + 1) return false
      if (dateItem.day !== date.getDate()) return false
      return true
    },
    cheackIndexId(customerList, firstList, cheackId) {
      const id = this.radios ? customerList.unitId : customerList.terapisNameId
      const resault = firstList.findIndex((object) => {
        return object.id === id
      })
      if (cheackId === resault) return true
      return false
    },
    calcReservationBoxPlaceY(openDate, reservationDate) {
      const diffHours = Math.abs(
        openDate.getHours() - reservationDate.getHours()
      )
      const diffMinitu =
        ((diffHours * 60 + reservationDate.getMinutes()) / 10) * 20
      return diffMinitu
    },
    calcReservationSmallBoxPlaceX(customerList, firstLists) {
      const id = this.radios ? customerList.terapisNameId : customerList.unitId
      const resault = firstLists.findIndex((object) => {
        return object.id === id
      })
      return resault
    },
    openReservationForm(data) {
      this.$emit('openFrom', data)
    },
  },
})
</script>
<style lang="scss" scoped>
.available-unit {
  position: absolute;
  background: #696969;
  width: 100%;
  height: 100%;
  z-index: 4;
  opacity: 0.3;
}
.box {
  overflow: auto;
  border: 1px solid #c3c3c3;
}
.header {
  .first-content {
    border-right: 1px solid #c3c3c3;
  }
  .second-content {
    border-right: 1px solid #c3c3c3;
    position: relative;
    &:last-child {
      border: none;
    }
  }
  .second-item {
    border-bottom: 1px solid #c3c3c3;
  }
  .second-row-text {
    color: #a0a0a0;
    font-size: 11px;
  }
  .first-row {
    height: 60px;
    text-align: center;
    background: #f5f6f8;
    max-height: 100%;
  }
  .second-row {
    height: 40px;
  }
}
.body {
  .time-content {
    border-right: 1px solid #c3c3c3;
  }
  .time-row {
    border-bottom: 1px solid #c3c3c3;
    &:last-child {
      border: none;
    }
  }
  .table-col {
    border-right: 1px solid #c3c3c3;
    position: relative;
    &:last-child {
      border: none;
    }
  }
  .table-row {
    border-bottom: 1px solid #c3c3c3;
    position: relative;
    &:last-child {
      border: none;
    }
  }
  .calender-time-table {
    height: 20px;
    border-bottom: 1px dotted #e2e2e2;
    &:last-child {
      border-bottom: none;
    }
  }
}
.icon-box {
  position: relative;
  .icon-btn {
    position: absolute;
    background: #f5f6f8;
    width: 22px;
    height: 22px;
    bottom: 0;
    right: 0;
    transform: translateY(-50%) translateX(50%);
    z-index: 10;
    border-radius: 50%;
    border: 1px solid #c3c3c3;
  }
}
.border-right {
  border-right: 1px solid #0000001a;
  text-align: center;
  &:last-child {
    border: none;
  }
}
.col-Width {
  width: var(---width);
  min-width: var(---width);
}
.second-col-Width {
  width: 40px;
  min-width: 40px;
}
.posion-rl {
  position: relative;
}
</style>
