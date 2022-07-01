<template>
  <div class="countdown py-6">
    <div class="calendar d-flex justify-center">
      <div class="unit">{{ countdown.days }} <span>Dias</span></div>
      <div class="unit">{{ countdown.hours }} <span>Horas</span></div>
      <div class="unit">{{ countdown.minutes }} <span>Minutos</span></div>
      <div class="unit">{{ countdown.seconds }} <span>Segundos</span></div>
    </div>
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
    .unit {
      display: flex;
      flex-direction: column;
      text-align: center;
      padding: 0 16px;
      color: #145da0;

      > span {
        font-size: 0.6em !important;
        text-transform: uppercase;
      }
    }
  }
}
</style>
