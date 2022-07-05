<template>
  <div class="is-absolute timeline-titles text-white">
    <!--
     TODO: Переписать на float, d-flex работает только на webOS 4
     -->
    <div class="d-flex">
      <div class="show-start-time">
        {{ startOfTheShow }}
      </div>
      <div class="time-until-show-start">
        Сейчас
      </div>
    </div>
    <div class="show-title">
      {{ title }}
    </div>
  </div>
</template>

<script>
import {formatInTimeZone} from "date-fns-tz";
import fromUnixTime from "date-fns/fromUnixTime";

export default {
  name: "TimelineTitles",
  props: {
    title: String,
    time: Number
  },
  data() {
    return {
      startOfTheShow : String
    }
  },
  methods: {
    parseTime() {
      this.startOfTheShow = formatInTimeZone(fromUnixTime(this.time), 'Europe/Moscow', 'HH:mm')
    }
  },
  watch: {
    time: function(val) {
      if (val) {
        this.parseTime()
      }
    }
  }
}
</script>

<style>
  .timeline-titles {
    top: 24px;
  }
  .show-start-time {
    font-size: 32px;
    font-weight: 400;
    line-height: 44px;
    letter-spacing: 0.0025em;
    text-align: left;
    margin-right: 23px;
  }

  .time-until-show-start {
    font-style: normal;
    font-weight: 500;
    font-size: 24px;
    line-height: 38px;
    letter-spacing: 0.0025em;
  }

  .show-title {
    font-style: normal;
    font-weight: 500;
    font-size: 40px;
    line-height: 48px;
    letter-spacing: -0.0005em;
    max-width: 765px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .timeline-future .show-title {
    font-style: normal;
    font-weight: 400;
    font-size: 32px;
    line-height: 44px;
    letter-spacing: 0.0025em;
    max-width: 320px;
  }
</style>