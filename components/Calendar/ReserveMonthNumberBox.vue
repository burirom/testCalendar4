<template>
  <div class="reserve-monthBox pt-10 pl-6 pr-6">
    <div class="reserve-monthBox-title">月間予約件数</div>
    <div class="pb-10">
      <div class="mb-3 pb-1 reserve-monthBox-left-text">
        <div
          v-for="(terapisList,
          index) in numberOfReservationsPerMonth.terapisLists"
          :key="index"
          class="numberItem d-flex"
        >
          <div class="d-flex align-end reserve-monthBox-name">
            {{ terapisList.terapisName }}
          </div>
          <div class="ml-auto d-flex">
            <div class="pl-3 pr-1 d-flex align-end reserve-monthBox-number">
              {{ terapisList.Number }}
            </div>
            <div class="reserve-monthBox-small-text d-flex align-end">件</div>
          </div>
        </div>
      </div>
      <div class="d-flex numberItem">
        <div class="reserve-monthBox-name d-flex align-end">合計</div>
        <div class="ml-auto d-flex">
          <div
            class="pl-3 pr-1 d-flex align-end reserve-monthBox-number sumary"
          >
            {{ numberOfReservationsPerMonth.TotalNumber }}
          </div>
          <div class="reserve-monthBox-small-text d-flex align-end">件</div>
        </div>
      </div>
    </div>
    <div>
      <div class="d-flex pb-1">
        <div class="reserve-monthBox-name d-flex align-end">空き枠</div>
        <div class="ml-auto d-flex">
          <div class="pl-3 pr-1 d-flex align-end reserve-monthBox-number">
            {{ numberOfReservationsPerMonth.emptyFrame }}
          </div>
          <div class="reserve-monthBox-small-text d-flex align-end">枠</div>
        </div>
      </div>
      <div class="d-flex mb-4 meter-box mr-1 ml-1">
        <div v-for="num of 10" :key="num.id">
          <div
            :class="num > activeFrame ? 'no-active-box' : 'active-box'"
          ></div>
        </div>
      </div>
      <div class="d-flex pb-1">
        <div class="reserve-monthBox-name d-flex align-end">稼働率</div>
        <div class="ml-auto d-flex">
          <div class="pl-3 pr-1 d-flex align-end reserve-monthBox-number">
            {{ numberOfReservationsPerMonth.OccupancyRate }}
          </div>
          <div class="reserve-monthBox-small-text d-flex align-end">%</div>
        </div>
      </div>
      <div class="d-flex mb-4 meter-box mr-1 ml-1">
        <div v-for="num of 10" :key="num.id">
          <div
            :class="num > activeOccupancyRate ? 'no-active-box' : 'active-box'"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    numberOfReservationsPerMonth: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      activeFrame: 0,
      activeOccupancyRate: 0,
    }
  },
  created() {
    this.setActiveOccupancyRate()
    this.setActiveFrame()
  },
  methods: {
    setActiveFrame() {
      this.activeFrame = Math.round(
        (this.numberOfReservationsPerMonth.emptyFrame /
          this.numberOfReservationsPerMonth.allFrame) *
          10
      )
    },
    setActiveOccupancyRate() {
      this.activeOccupancyRate = Math.round(
        this.numberOfReservationsPerMonth.OccupancyRate / 10
      )
    },
  },
}
</script>
<style lang="scss" scoped>
.reserve-monthBox {
  overflow-y: auto;
  max-height: 540px;
  height: 540px;
  max-width: 100%;
  .reserve-monthBox-title {
    text-align: center;
  }
  .reserve-monthBox-left-text {
    border-bottom: 2px solid #a0a0a0;
  }
  .reserve-monthBox-name {
    font-size: 15px;
    color: #333;
  }
  .reserve-monthBox-small-text {
    color: #a0a0a0;
  }
  .reserve-monthBox-number {
    color: #004adb;
    font-size: 22px;
  }
  .sumary {
    color: #d90000;
  }
}
.meter-box {
  justify-content: space-between;
}
.no-active-box {
  height: 13px;
  width: 8px;
  background: #fff;
  border: 1px solid #004adb;
}
.active-box {
  height: 13px;
  width: 8px;
  background: #004adb;
  border: 1px solid #004adb;
}
</style>
