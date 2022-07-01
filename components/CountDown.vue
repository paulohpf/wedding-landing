<template>
  <div class="countdown py-4 d-flex justify-center">
    <v-row class="calendar pa-0 ma-0 justify-center">
      <v-col cols="2" class="unit ma-2 pa-2">{{ countdown.days }} <span>Dias</span></v-col>
      <v-col cols="2" class="unit ma-2 pa-2">{{ countdown.hours }} <span>Horas</span></v-col>
      <v-col cols="2" class="unit ma-2 pa-2">{{ countdown.minutes }} <span>Minutos</span></v-col>
      <v-col cols="2" class="unit ma-2 pa-2">{{ countdown.seconds }} <span>Segundos</span></v-col>
    </v-row>
  </div>
</template>

<script>
import { differenceInSeconds } from 'date-fns'

export default {
  name: 'CountDown',
  props: {
    deadline: {
      type: Date,
      required: true,
    },
  },
  data() {
    return { countdown: {} }
  },
  mounted() {
    this.getCoundown()
  },
  methods: {
    getCoundown() {
      const currentTime = new Date().getTime()
      const diffInSeconds = differenceInSeconds(this.deadline, currentTime)
      const ONE_DAY = 60 * 60 * 24
      const ONE_HOUR = 60 * 60
      const ONE_MINUTE = 60

      if (diffInSeconds <= 1) {
        this.countdown = {
          days: 0,
          hours: 0,
          minutes: 0,
          seconds: 0,
        }
        return
      }

      const days = Math.floor(diffInSeconds / ONE_DAY)
      const hours = Math.floor((diffInSeconds - days * ONE_DAY) / ONE_HOUR)
      const minutes = Math.floor(
        (diffInSeconds - days * ONE_DAY - hours * ONE_HOUR) / ONE_MINUTE
      )
      const seconds =
        diffInSeconds - days * ONE_DAY - hours * ONE_HOUR - minutes * ONE_MINUTE

      this.countdown = {
        days,
        hours,
        minutes,
        seconds,
      }

      setTimeout(() => {
        this.getCoundown()
      }, 1000)
    },
  },
}
</script>

<style lang="scss" scoped>
.countdown {
  .calendar {
    max-width: 400px;
    .unit {
      display: flex;
      flex-direction: column;
      text-align: center;
      margin: 0 16px;
      padding: 8px;
      color: #eaf8fe;
      background: rgb(114, 131, 110);
      border-radius: 2px;
      font-weight: 500;

      > span {
        font-size: 0.5em !important;
        text-transform: uppercase;
      }
    }
  }
}
</style>
