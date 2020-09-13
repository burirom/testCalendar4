<template>
  <div class="box" style="width: 100%">
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
        <div class="second-item first-row d-flex align-center justify-center">
          {{ firstList.dayOfWeekText }}
        </div>
        <!-- close secondList -->
        <div
          v-if="!cheackActiveSecondList(firstIndex)"
          class="second-item second-row icon-box"
        >
          <div
            v-if="radios"
            class="first-row second-row-text d-flex align-center justify-center"
          >
            担当者表示
          </div>
          <div
            v-else
            class="first-row second-row-text d-flex align-center justify-center"
          >
            ユニット表示
          </div>
          <v-icon class="icon-btn" @click="openSecondList(firstIndex)">
            mdi-chevron-right
          </v-icon>
        </div>
        <!-- open secondList -->
        <div v-else class="d-flex icon-box second-item second-row">
          <div
            v-for="(secondList, secondListIndex) in secondLists"
            :key="secondListIndex"
            class="border-right second-col-Width d-flex align-center justify-center"
          >
            <div v-if="radios" class="second-row-text">
              {{ secondList.terapisName.substring(0, 1) }}
            </div>
            <div v-else class="second-row-text">
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
      <div class="time-content d-flex" style="min-width: 200px">
        <slot name="summaryItem" />
        <div>
          <div
            v-for="(time, timeIndex) in times"
            :key="timeIndex"
            class="time-row"
          >
            <p class="calender-time-table ma-0 pl-2 pr-2">{{ time }}</p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          </div>
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
                v-if="DateComparison(firstList, customerList.start)"
                :row-width="40"
                :customer-info="customerList"
                :box-place-x="
                  calcReservationSmallBoxPlaceX(customerList, secondLists)
                "
                :box-place-y="
                  calcReservationBoxPlaceY(
                    businessTime.open,
                    customerList.start
                  )
                "
              />
            </div>
          </div>
          <div v-else-if="!cheackActiveSecondList(firstIndex)">
            <div
              v-for="(customerList, index) in customerLists"
              :key="`first-${index}`"
              class="item-handle"
            >
              <ReservationBox
                v-if="DateComparison(firstList, customerList.start)"
                :row-width="100"
                :customer-info="customerList"
                :box-place-x="0"
                :box-place-y="
                  calcReservationBoxPlaceY(
                    businessTime.open,
                    customerList.start
                  )
                "
              />
            </div>
          </div>
          <div
            v-for="(time, timeIndex) in times"
            :key="timeIndex"
            class="d-flex table-row"
          >
            <!-- close secondList -->
            <div
              v-if="!cheackActiveSecondList(firstIndex)"
              class="table-row"
              style="width: 100%"
            >
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
              <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            </div>
            <!-- open secondList -->
            <div v-else class="d-flex table-row testssssss">
              <div
                v-for="(secondList, secondListIndex) in secondLists"
                :key="secondListIndex"
                class="border-right second-col-Width"
              >
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
    calcReservationSmallBoxPlaceX(customerList, secondList) {
      const id = this.radios ? customerList.terapisNameId : customerList.unitId
      const resault = secondList.findIndex((object) => {
        return object.id === id
      })
      return resault
    },
  },
})
</script>
<style lang="scss" scoped>
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
    &:last-child {
      border: none;
    }
  }
  .second-item {
    border-bottom: 1px solid #c3c3c3;
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
  .second-row-text {
    font-size: 11px;
    color: #a0a0a0;
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
    &:last-child {
      border: none;
    }
  }
  .table-row {
    border-bottom: 1px solid #c3c3c3;
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
    z-index: 999;
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
