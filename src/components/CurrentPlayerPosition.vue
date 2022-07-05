<template>
  <div class="current-player-position is-absolute">
    {{ time }}
  </div>
</template>

<script>

import fromUnixTime  from 'date-fns/fromUnixTime'
import { formatInTimeZone } from 'date-fns-tz'


export default {
  name: "CurrentPlayerPosition",
  props: {
    currentTime : Number
  },
  data() {
    return {
      time: String
    }
  },
  methods: {
    parseTime() {
      this.time = formatInTimeZone(fromUnixTime(this.currentTime), 'Europe/Moscow', 'HH:mm:ss')
    }
  },
  watch: {
    currentTime: function(val) {
      if (val) {
       this.parseTime()
      }
    }
  }
}
</script>

<style scoped>
  .current-player-position {
    width: 204px;
    height: 44px;
    text-align: center;
    background: rgba(0, 0, 0, 0.7);
    border-radius: 32px;
    font-weight: 400;
    font-size: 32px;
    line-height: 44px;
    letter-spacing: 0.0025em;
    color: rgba(255, 255, 255, 0.8);
    right:-102px;
    top: -68px;
  }
</style>