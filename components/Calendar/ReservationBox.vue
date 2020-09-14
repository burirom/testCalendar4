<template>
  <div class="reservation-box" :style="boxStyles">
    <div
      :style="boxStyles"
      class="reservation-content pa-2 mt-1 mb-1"
      :class="[
        customerInfo.nomination ? 'nomination-box' : 'no-nomination-box',
        boxShadoweCheack ? 'box-sadow' : 'box-no-sadow',
      ]"
      @click="boxClick"
    >
      <div v-if="rowWidth >= 100" class="text">
        <div>
          {{ customerInfo.name.lastName }}{{ customerInfo.name.firstName }}様
        </div>
        <div v-if="customerInfo.customerNumberOfPeople != 0">
          他{{ customerInfo.customerNumberOfPeople }}名
        </div>
        <div class="text">{{ reseversionTimeText }}</div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    rowWidth: {
      type: Number,
      default: 100,
    },
    boxPlaceX: {
      type: Number,
      default: 0,
    },
    boxPlaceY: {
      type: Number,
      default: 0,
    },
    customerInfo: {
      type: Object,
      default: null,
    },
    boxShadow: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      diffMinute: 0,
    }
  },
  computed: {
    boxStyles(): object {
      return {
        '---width': this.rowWidth + 'px',
        '---height': Math.round(this.diffMinute / 10) * 20 - 2 + 'px',
        '---boxPlaceX': this.boxPlaceX * this.rowWidth + 'px',
        '---boxPlaceY': this.boxPlaceY + 'px',
      }
    },
    reseversionTimeText(): string {
      const startMinutes = this.customerInfo.start.getMinutes()
      const endMinutes = this.customerInfo.end.getMinutes()
      const startHours = this.customerInfo.start.getHours()
      const endHours = this.customerInfo.end.getHours()
      if (startMinutes === 0 || endMinutes === 0)
        return (
          startHours + ':' + startMinutes + '~' + endHours + ':' + endMinutes
        )
      return startHours + ':' + startMinutes + '~' + endHours + ':' + endMinutes
    },
    boxShadoweCheack() {
      if (!this.boxShadow) return false
      if (this.rowWidth < 100) return false
      return true
    },
  },
  created() {
    this.diffMinute = this.calcDiffTime(
      this.customerInfo.start,
      this.customerInfo.end
    )
  },
  methods: {
    // 時間の差分の計算(分)
    calcDiffTime(startTime: Date, endTime: Date): number {
      const diff = startTime.getTime() - endTime.getTime() + 30
      const diffMinute = Math.abs(diff) / (60 * 1000) + startTime.getMinutes()
      return Math.round(diffMinute)
    },
    boxClick() {
      this.$emit('boxClick')
    },
  },
})
</script>
<style lang="scss" scoped>
.reservation-box {
  position: absolute;
  width: var(---width);
  padding: 2px;
}
.reservation-content {
  margin: 1px;
  position: relative;
  height: var(---height);
  left: var(---boxPlaceX);
  top: var(---boxPlaceY);
  z-index: 10;
}
.nomination-box {
  background: #6e9eff;
  border: 1px solid #6e9eff;
  border-radius: 3px;
  .text {
    color: #fff;
    font-size: 13px;
  }
}
.no-nomination-box {
  background: #e5eeff;
  border: 2px solid #6e9eff;
  border-radius: 3px;
  .text {
    color: #6e9eff;
    font-size: 13px;
  }
}
.box-sadow {
  box-shadow: 3px 3px 5px #000;
}
.box-no-sadow {
  box-shadow: none;
}
</style>
