<template>
  <div class="player-menu is-relative">
    <div class="overlay is-absolute"></div>
    <TimelineGroup/>
  </div>
</template>

<script>
import TimelineGroup from './components/TimelineGroup'

export default {
  name: 'App',
  components: {
    TimelineGroup
  }
}
</script>

<style>
body {
  width: 1920px;
  height: 1080px;
}

.is-absolute {
  position: absolute;
}

.is-relative {
  position: relative;
}

.text-white {
  color: #EBEBEB;
}

.d-flex {
  display: flex;
}

.player-menu {
  top: 0;
  left: 0;
  width: 1920px;
  height: 1080px;
}
.overlay {
  width: 1920px;
  height: 1080px;
  left: 0;
  top: 0;
  background: linear-gradient(180deg, #000000 0%, rgba(0, 0, 0, 0) 100%);
  transform: matrix(1, 0, 0, -1, 0, 0);
}
</style>
