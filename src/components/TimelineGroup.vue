<template>
  <div class="timeline-wrapper is-absolute" >
    <div v-if="state.isError" >
     <h1 class="text-white">
       Ошибка получения ТВ программы
     </h1>
    </div>
    <!--
        TODO: Переписать на float, d-flex работает только на webOS 4
    -->
    <div class="d-flex timeline-group" v-if="state.isReady">
      <div class="timeline timeline-past">
        <div class="timeline-bar" style="width: 100%" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
      </div>
      <div class="timeline timeline-current is-relative">
        <div class="timeline-bar is-relative" role="progressbar" :style="{'width': programOnScreen.time.progress + '%' }" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100">
         <CurrentPlayerPosition v-bind:currentTime="MoscowTime" />
        </div>
        <TimelineTitles
            :title="programOnScreen.title.current"
            :time="programOnScreen.time.current"
        />
      </div>
      <div class="timeline timeline-future is-relative">
        <div class="timeline-bar" role="progressbar" style="width:0" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
        <TimelineTitles/>
      </div>
    </div>
  </div>

</template>

<script>
import TimelineTitles from "@/components/TimelineTitles";
import CurrentPlayerPosition from "@/components/CurrentPlayerPosition";


export default {
  name: 'TimelineGroup',
  components: {
    TimelineTitles,
    CurrentPlayerPosition
  },
  data() {
    return {
      state: {
        isReady : false,
        isError: false,
      },
      TvProgram: String,
      MoscowTime: Number,
      timer: null,
      programOnScreen: {
        currentIndex: Number,
        time : {
          current: String,
          progress: Number,
          future : String,
        },
        title: {
          current : String,
          future : String,
        },
        when: {
          current: String,
          future: String,
        }
      }
    }
  },
  created() {

    /**
     * TODO : Сделать определение московского времени из formatInTimeZone(new Date)
     */

    this.fetchData("http://worldtimeapi.org/api/timezone/Europe/Moscow").then((data)=>{
       this.MoscowTime =  data?.unixtime;
    }).then(()=>{
      this.fetchData("../programs.json").then((data)=>{
        this.TvProgram = data;
      }).then(()=>{
        this.findDate();
      })
    }).catch((e)=> {
      console.error(e.toString());
      this.initError(true)
    })
  },
  methods: {
   async fetchData(url){
     const response = await fetch(url, {
       method: "get"
     });
     return response.json();
    },
    findDate() {
      this.TvProgram?.programs.every((item, index)=>{
        if (item.scheduleInfo.start <= this.MoscowTime && item.scheduleInfo.end >= this.MoscowTime) {
          this.programOnScreen.currentIndex = index;
          this.renderScreen();
          return false;
        }
        return true
      });

      if (typeof this.programOnScreen.currentIndex === 'function') {
        this.initError(true)
      } else {
       this.initError(false)
      }

    },
    renderScreen() {
      const item = this.TvProgram?.programs[this.programOnScreen.currentIndex];
      const futureItem = this.TvProgram?.programs[this.programOnScreen.currentIndex + 1];

      this.programOnScreen.title.current = item.metaInfo.title
      this.programOnScreen.time.current = item.scheduleInfo.start

      this.programOnScreen.title.future = futureItem.metaInfo.title;
      this.programOnScreen.time.future = futureItem.metaInfo.start;


      this.renderProgressBar();
      this.initTimer();
    },
    initError(error) {
     [this.state.isError, this.state.isReady] = [error, !error];
    },
    renderProgressBar() {

     const item = this.TvProgram?.programs[this.programOnScreen.currentIndex];
     const totalProgress = item.scheduleInfo.end - item.scheduleInfo.start;
     const timeWatched = this.MoscowTime - item.scheduleInfo.start;
    /*
      TODO: Логика высчитывания прогресс бара требует тестов
     */
     this.programOnScreen.time.progress = (timeWatched / totalProgress) * 100;
    },
    initTimer() {
     this.timer =  setInterval(() => {
        this.MoscowTime++;
      }, 1000);
    }
  }
}
</script>

<style scoped>
  .timeline-wrapper {
    bottom: 261px;
  }
  .timeline-group {
    font-family: 'Roboto', sans-serif;
  }

  .timeline {
    background: rgba(235, 235, 235, 0.2);
    border-radius: 8px;
    height: 12px;
    margin-right: 24px;
  }

  .timeline-bar {
    background: #FFC000;
    border-radius: 8px;
    height: 100%;

  }
  .timeline-past {
    width: 249px;
  }

  .timeline-past .timeline-bar {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }

  .timeline-current {
    width: 1280px;
  }

  .timeline-future {
    width: 340px;
  }
</style>
