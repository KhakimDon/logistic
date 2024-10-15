<script setup lang="ts">
import { ref, computed } from 'vue';

const data = ref([
  {
    id: 1,
    eventTitle: "Bigfish",
    eventTime: 18000,
    date: "13 Oct"
  },
  {
    id: 2,
    eventTitle: "Something",
    eventTime: 7200,
    date: "14 Oct"
  },
])

const events = ref([
  {
    y: 60,
    x1: 0,
    x2: 70,
    title: "On the way"
  },
  {
    y: 150,
    x1: 0,
    x2: 130,
    title: "Eating"
  },
  {
    y: 320,
    x1: 0,
    x2: 395,
    title: "Sleeping"
  },
  {
    y: 230,
    x1: 0,
    x2: 460,
    title: "On the way",
  },
  {
    y: 60,
    x1: 0,
    x2: 128,
    title: "Chill out"
  },
  {
    y: 150,
    x1: 0,
    x2: 130,
    title: "Do something"
  },
  {
    y: 60,
    x1: 0,
    x2: 790,
    title: "On the way"
  },
  {
    y: 245,
    x1: 0,
    x2: 130,
    title: "Sleeping"
  },


])


const more_infos = ref([
  {
    date: "13 Oct",
    infos: [
      {
        time: "00:00 - 01:00",
        title: "On the way",
      },
      {
        time: "01:00 - 03:00",
        title: "Eating",
      },
      {
        time: "00:00 - 01:00",
        title: "Sleeping",
      },
      {
        time: "03:00 - 09:00",
        title: "On the way",
      },
      {
        time: "09:00 - 16:00",
        title: "On the way",
      },
    ]
  },
  {
    date: "14 Oct",
    infos: [
      {
        time: "00:00 - 01:00",
        title: "Do something",
      },
      {
        time: "01:00 - 03:00",
        title: "Eating",
      },
      {
        time: "00:00 - 01:00",
        title: "Do something",
      },
      {
        time: "03:00 - 09:00",
        title: "On the way",
      },
      {
        time: "09:00 - 16:00",
        title: "Do something",
      },
    ]
  },
])


const showInfo = ref(false)
const tooltipX = ref(0)
const tooltipY = ref(0)
const infos = ref('')

const showDay = ref(0)

const handleMouseMove = (event: any) => {
  showInfo.value = true
  tooltipX.value = event.clientX + 15
  tooltipY.value = event.clientY + 15

}


const tooltipStyles = computed(() => ({
  top: `${tooltipY.value}px`,
  left: `${tooltipX.value}px`,
  zIndex: 1000
}))


const onScroll = (event) => {
  const scrollLeft = event.target.scrollLeft
  const how_many_days = 2
  let scroll_pixeel = 1200


  const arr = ref([])
  for (let i = 0; i < how_many_days; i++) {
    arr.value.push(scroll_pixeel)
    scroll_pixeel *= 2
  }
  
  
  showDay.value = findIndex(scrollLeft, arr.value)
  
}

function findIndex(value: any, array: any) {
  
  for (let i = 0; i < array.length; i++) {
    if (value < array[i]) {
      return i;
      ;
    }
  }
  return array.length;
}

</script>

<template>
  <div class="main">
    <h1>Boost</h1>
    <div style="display: flex; gap: 10px">
      <div @scroll="onScroll" class="wrapper">

        <div v-for="event of data" :key="event.id" class="event">
          <div class="hours">
            <span class="date">{{ event.date }}</span>
          </div>

          <div class="blocks">
            <div class="block-wrp" v-for="item of 24" :key="item.id">
              <span class="showHour" v-if="item != 24 && item != 12">{{ item }}</span>
              <span class="showHour" style="font-weight: 700;" v-if="item == '12'">N</span>
              <div class="cubes" :class="{ red: item % 2 == 0 }" v-for="cubes of 4" :key="cubes.id">
                <div class="line"></div>
                <div class="line"></div>
                <div class="line"></div>
                <div class="line"></div>
                <div class="line"></div>
              </div>
            </div>
          </div>

        </div>
        <div class="wrp">

          <div @mousemove="handleMouseMove" @mouseenter="infos = item" @mouseleave="showInfo = false"
            class="event-block" v-for="(item, index) of events" :key="item.id">

            <svg :style="`width: ${item.x2 - item.x1}px; height: 100%`">
              <line :x1="item.x1" :y1="item.y" :x2="item.x2" :y2="item.y" stroke="#465A95" stroke-width="5" />
              <line v-if="events[index + 1]" :x1="item.x2" :y1="item.y" :x2="item.x2" :y2="events[index + 1].y + 1"
                stroke="#465A95" stroke-width="2" />
            </svg>



          </div>
        </div>


      </div>
      <div class="fixed-data">
        <h2>Information:</h2>
        <div>
          <div>
            <div class="dates">Date: {{ more_infos[showDay].date }}</div>
            <ul>
              <li class="text-b" v-for="active of more_infos[showDay].infos" :key="active.id"><b class="text-a">{{
                  active.time }}</b> <br> {{ active.title }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showInfo" :style="tooltipStyles" class="tooltip">
      <p>{{ infos.title }}</p>
    </div>
  </div>
</template>

<style>
.text-a {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.text-b {
  margin-bottom: 5px;
  border-bottom: 1px solid red;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.dates {
  margin-bottom: 5px;
  border-bottom: 1px solid silver;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 400;
}

.fixed-data {
  padding: 10px;
  width: 250px;
  height: 400px;
  border-radius: 15px;
  background: white;
}

.fixed-data h2 {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.tooltip {
  background: white !important;
  padding: 10px !important;
  border-radius: 10px !important;
  position: absolute !important;
  border-top: 10px solid #465A95 !important;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.415) !important;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  pointer-events: none;
}

.event-block {
  position: relative;
  margin-top: -2px;
  height: calc(100% + 2px);
  transition-duration: .3s;
}


.event-block:hover {
  background: #465a9580;
}

.wrp {
  display: flex;
  padding-left: 50px;
  z-index: 999;
  width: 100%;
  left: 0;
  top: 30px;
  height: calc(100% - 30px);
  position: absolute;
  /* background: rgba(255, 0, 0, 0.437); */
}

.showHour {
  top: -23px;
  font-weight: 00;
  right: 0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  transform: translateX(50%);
  position: absolute;
}

.red {
  background: #F8F9FB !important;
}

.line {
  height: 20px;
  width: 1px;
  background: #DCDCDC;
}

.line:nth-child(3) {
  height: 24px;
  width: 1px;
  background: #DCDCDC;
}

.line:nth-child(4),
.line:nth-child(2) {
  height: 14px;
  width: 1px;
  background: #DCDCDC;
}

.line:nth-child(1),
.line:nth-child(5) {
  height: 7px;
  width: 1px;
  background: #DCDCDC;
}


.cubes {
  display: flex;
  justify-content: space-around;
  align-items: flex-end;
  background: #FFFFFF;
  height: 48px;
  flex: 1;
}

.block-wrp {
  position: relative;
  gap: 1px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.blocks {
  gap: 1px;
  height: 350px;
  display: flex;
  background: #DCDCDCed;
}

.hours {
  display: flex;
  background: white;
  height: 30px;
  margin-top: -1px;
  border: 1px solid #DCDCDC;
}

* {
  padding: 0;
  margin: 0;
  list-style-type: none;
  box-sizing: border-box;
  caret-color: transparent;
}

.date {
  display: block;
  background: white;
  width: max-content;
  transform: translateX(-50%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 700;
  font-size: 20px;
}

.event:first-child {
  border-left: 5px solid #465A95;
}

.event {
  border-right: 2.5px solid #465A95;
  border-left: 2.5px solid #465A95;
  position: relative;
  display: block;
  height: 100%;
  min-width: 100% !important;
  background: #33333354;
}


.main {
  padding: 30px;
  height: 100vh;
  background: #465A95;
}

h1 {
  color: white;
  margin-bottom: 20px;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.wrapper {
  position: relative;
  padding-left: 50px;
  flex-wrap: nowrap;
  display: flex;
  border-radius: 10px;
  border: 1px solid #EEEEEE;
  height: 400px;
  width: 100%;
  max-width: 1920px;
  overflow-x: scroll;
  overflow-y: hidden;
  background: white;
}

::-webkit-scrollbar {
  border-radius: 60px;
  background: #E2E6F3;
}

/* ::-webkit-scrollbar-button — кнопки направления на полосе прокрутки.
::-webkit-scrollbar-track — пустое пространство под индикатором прокрутки.  */
::-webkit-scrollbar-thumb {
  border-radius: 60px;
  background: #6576A8;
}
</style>
