<template>
  <v-dialog
    :value="value"
    persistent
    scrollable
    max-width="400px"
    transition="slide-x-reverse-transition"
    @input="$emit('input')"
  >
    <v-card>
      <v-toolbar color="secondary" dark flat>
        <v-toolbar-title>{{ getDateToFormat }}</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn icon large @click.native="$emit('input')">
          <v-icon>mdi-close</v-icon></v-btn
        >
      </v-toolbar>
      <v-card-actions>
        <v-btn color="#DB4C00">
          <!-- {{
          $t('reservations.buttons.reservationStatus.tentative')
        }} -->
          仮予約
        </v-btn></v-card-actions
      ><v-card-text style="height: 100vh">
        <v-select
          v-model="form.Id"
          :items="namesList"
          :label="/* $t('reservations.form.name') */ '名前'"
          item-text="name"
          item-value="id"
          required
        ></v-select>
        <v-text-field
          v-model="form.name_reading"
          :label="/* $t('reservations.form.nameReading')*/ 'フリガナ'"
          required
        ></v-text-field>
        <v-radio-group v-model="form.gender" row>
          <v-radio :label="/* $t('common.male')*/ '男'" value="male"></v-radio>
          <v-radio
            :label="/*$t('common.female')*/ '女'"
            value="female"
          ></v-radio>
          <template v-slot:label>
            性別
            <!-- {{ $t('common.gender') }} -->
          </template>
        </v-radio-group>
        <v-row>
          <v-col>
            <v-select
              v-model="form.terapisNameId"
              :items="therapists"
              item-value="id"
              background-color="secondary"
              dark
              item-text="terapisName"
              :label="/*$t('reservations.form.therapist')*/ '担当者'"
            ></v-select> </v-col
          ><v-col cols="4">
            <v-checkbox
              v-model="form.nomination"
              :label="/*$t('reservations.form.therapistSelected')*/ '担当者'"
            ></v-checkbox></v-col
        ></v-row>
        <v-select
          v-model="form.courseId"
          :items="courses"
          item-text="courseName"
          item-value="id"
          background-color="secondary"
          dark
          :label="/*$t('reservations.form.course')*/ 'コース'"
          ><template v-slot:items><span>dsfsf</span></template></v-select
        >
        <!-- unit config -->
        <v-row v-for="(unitList, index) in unitLists" :key="index">
          <v-col cols="12">
            <v-select
              v-model="unitList.unitId"
              :items="units"
              item-text="unitName"
              item-value="id"
              background-color="secondary"
              dark
              :label="/* $t('reservations.form.unit')*/ 'ユニット'"
            ></v-select>
          </v-col>
          <v-col cols="5">
            <v-menu>
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="unitList.date"
                  :label="/*$t('reservations.form.reservationDate')*/ '予約日'"
                  prepend-icon="mdi-calendar-check"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker v-model="unitList.date" no-title></v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="5">
            <v-menu ref="menu" :close-on-content-click="false">
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="unitList.start"
                  :label="/*$t('reservations.form.reservationTime')*/
                  '予約時間'"
                  prepend-icon="mdi-clock"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-time-picker
                v-model="unitList.start"
                full-width
              ></v-time-picker>
            </v-menu>
            <v-menu ref="menu" :close-on-content-click="false">
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="unitList.end"
                  :label="/*$t('reservations.form.reservationTime')*/
                  '予約時間'"
                  prepend-icon="mdi-clock"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-time-picker v-model="unitList.end" full-width></v-time-picker>
            </v-menu>
          </v-col>
          <v-col v-if="form.unitLists.length === index" cols="2">
            <v-btn icon @click="addUnit">
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-text-field
          v-model="form.receptionist_comments"
          :label="/*$t('reservations.form.receptionistComments')*/ 'コメント'"
          outlined
        ></v-text-field>
        <v-text-field
          v-model="form.patient_comments"
          :label="/*$t('reservations.form.patientComments')*/ 'コメント'"
          outlined
        ></v-text-field
      ></v-card-text>
      <v-card-actions
        ><v-btn
          height="50"
          class="py-4 px-5"
          border-radius="3px"
          depressed
          color="primary"
          @click="submit"
        >
          保存
          <!-- {{ $t('common.buttons.add') }} --> </v-btn
        ><v-spacer></v-spacer
        ><v-btn icon large><v-icon>mdi-delete</v-icon></v-btn></v-card-actions
      ></v-card
    >
  </v-dialog>
</template>
<!--<i18n src="~/i18n/common.yaml" lang="yaml">
</i18n>
<i18n src="~/i18n/reservations.yaml" lang="yaml">
</i18n>-->
<script type="ts">
export default {
  props: {
    value: {
      type: Boolean,
      required: true,
    },
    names: {
      type: Array,
      required: true,
      default: null
    },
    therapists: {
      type: Array,
      required: true,
      default: null
    },
    units: {
      type: Array,
      required: true,
      default: null
    },
    courses: {
      type: Array,
      required: true,
      default: null
    },
    date: {
      type: Date,
      required: true,
      default: new Date()
    },
    customerList: {
      type: Object,
      default: null
    }
  },
  data() {
    const reservationForm = {
      Id: '',
      name: '',
      name_reading: '',
      gender: '',
      terapisNameId: '',
      unitLists: [],
      courseId: '',
      receptionist_comments: '',
      patient_comments: '',
      nomination: false,
      dataType: 0, // 0: add  1: update  3: daleate
    }
    return {
      form: Object.assign({}, reservationForm),
      weeks: ['日', '月', '火', '水', '木', '金', '土'],
      id: null,
      unitLists: [
        {
          unitId: '',
          start: '',
          end: '',
          date: this.$moment(this.date).format('YYYY-MM-DD')
        }
      ],
    }
  },
  computed: {
     getDateToFormat () {
      const result = this.$moment(this.date).format('YYYY/MM/DD') +
        `(${this.weeks[this.date.getDay()]})`
        return result
     },
    namesList () {
      const result = this.names.map(function (item) {
        return {
           id: item.customerId,
           name: item.name.lastName + item.name.firstName,
           nameReading: item.name.lastNameReading + item.name.firstNameReading
        }
      })
      return result
    }
  },
  watch: {
    id () {
      this.setCustomerStatus()
    },
  },
  created () {
     if(!this.customerList) return false
     this.setCustomerListToForm()
     this.setCustomerStatus()
  },
  methods: {
    setCustomerListToForm () {
       this.form.dataType = 1
       this.form.Id = this.customerList.customerId
       this.form.nomination = this.customerList.nomination
       this.form.terapisNameId = this.customerList.terapisNameId
       this.form.courseId = this.customerList.courseId
       this.unitLists[0].unitId = this.customerList.unitId
       this.unitLists[0].start = this.$moment(this.customerList.start).format('HH:mm')
       this.unitLists[0].end = this.$moment(this.customerList.end).format('HH:mm')
       this.unitLists[0].date = this.$moment(this.customerList.start).format('YYYY-MM-DD')

    },
    resetForm() {
      this.form = Object.assign({}, this.reservationForm)
      this.$refs.form.reset()
    },
    submit() {
      this.pushUnitLists(this.form.unitLists)
    },
    setCustomerStatus () {
      const id = this.form.Id
        this.names.forEach((items) =>{
          if(items.customerId === id) {
            this.form.Id = items.customerId
            this.form.name = items.name.lastName + items.name.firstName
            this.form.name_reading = items.name.lastNameReading + items.name.firstNameReading
            this.form.gender = items.gender === 1 ? 'male' : 'female'
          }
        });
    },
    addUnit () {
      this.unitLists.push({
          unitId: '',
          start: '',
          end: '',
          date: this.$moment(this.date).format('YYYY-MM-DD')
      })
    },
    pushUnitLists (pushList) {
        this.unitLists.forEach((items) =>{
            const date = this.$moment(items.date).format('YYYY/MM/DD')
            pushList.push({
               unitId: items.unitId,
               start: `${date}/${items.start}`,
               end: `${date}/${items.end}`
            })
        });
    }
  },
}
</script>
<style scoped>
.v-dialog__content {
  align-items: flex-end;
  justify-content: flex-end;
}
</style>
