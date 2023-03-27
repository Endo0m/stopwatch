<template>
  <div class="bg">
    <div
      v-for="stopwatch in stopwatches"
      :key="stopwatch.id"
      class="stopwatch-container"
    >
      <div class="stopwatch-header">
        <div
          class="stopwatch-time"
          :class="{ running: stopwatch.isRunning }"
          v-text="formatTime(stopwatch.time)"
        ></div>
        <div class="stopwatch-actions">
          <div
            class="stopwatch-action"
            :class="{ 'stopwatch-action-start': !stopwatch.isRunning }"
            @click="toggleStopwatch(stopwatch)"
          >
            <span v-if="stopwatch.isRunning">
              <img src="./assets/pause.svg"
            /></span>
            <span v-else> <img src="./assets/play.svg" /></span>
          </div>
          <div
            class="stopwatch-action"
            @click="resetStopwatch(stopwatch)"
            :class="{ running: stopwatch.isRunning }"
          >
            <img
              :src="
                stopwatch.isRunning
                  ? 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAlSURBVHgB7cxBDQAACAOxgX/PMBH7LLkK6JwpaBVGSEhISNgSPrMVBCQ1YTjHAAAAAElFTkSuQmCC'
                  : 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAnSURBVHgB7cxBDQAACAOxgX8Nswom9llyFdCxfQpahRESEhIStoQPg3YDAf73BxwAAAAASUVORK5CYII='
              "
              @click="toggle"
            />
          </div>
        </div>
      </div>
    </div>
    <button class="stopwatch-container" @click="addStopwatch">
      <img src="./assets/plus.png" />
    </button>
  </div>
</template>

<script>
import { ref } from "vue";
import "../src/assets/reset.css";

export default {
  setup() {
    const stopwatches = ref([
      { id: 1, name: "Stopwatch 1", time: 0, isRunning: false, interval: null },
      { id: 2, name: "Stopwatch 2", time: 0, isRunning: false, interval: null },
    ]);
    let nextId = 3;

    function addStopwatch() {
      stopwatches.value.push({
        id: nextId++,
        name: `Stopwatch ${nextId}`,
        time: 0,
        isRunning: false,
        interval: null,
      });
    }

    function toggleStopwatch(stopwatch) {
      if (stopwatch.isRunning) {
        clearInterval(stopwatch.interval);
        stopwatch.interval = null;
      } else {
        stopwatch.interval = setInterval(() => ++stopwatch.time, 10);
      }
      stopwatch.isRunning = !stopwatch.isRunning;
    }

    function resetStopwatch(stopwatch) {
      clearInterval(stopwatch.interval);
      stopwatch.interval = null;
      stopwatch.isRunning = false;
      stopwatch.time = 0;
    }

    function formatTime(milliseconds) {
      const minutes = Math.floor(milliseconds / 60000);
      const seconds = Math.floor(milliseconds / 1000) % 60;
      const centiseconds = Math.floor(milliseconds / 10) % 100;
      return `${minutes > 0 ? minutes + ":" : ""}${
        seconds < 10 ? "0" : ""
      }${seconds}.${centiseconds < 10 ? "0" : ""}${centiseconds}`;
    }

    return {
      stopwatches,
      addStopwatch,
      toggleStopwatch,
      resetStopwatch,
      formatTime,
    };
  },
};
</script>

<style scoped>
@font-face {
  font-family: "Gotham Pro";
  src: url("@/font/gothampro.ttf");
}

.bg {
  height: 100vh;
  padding: 72px 212px 102px;
  display: grid;
  grid-template-columns: 240px 240px 240px;

  column-gap: 10px;
  row-gap: 1em;
  z-index: 9;
}
@media (max-width: 1060px) {
  .bg {
    padding: 72px 135px 102px;
    grid-row: 1rem;
    display: grid;
    grid-template-columns: 240px 240px 240px;
  }
}
@media (max-width: 820px) {
  .bg {
    padding: 10px 20px;
    display: grid;
    grid-template-columns: 240px 240px;

    gap: 3px;
    background: #353638;
  }
}
@media (max-width: 700px) {
  .bg {
    height: 100vh;
    width: 100vh;
    padding: 72px 135px 102px;
    grid-row: 1rem;
    display: grid;
    grid-template-columns: 240px;

    background: #353638;
  }
}
.stopwatch-container {
  height: 120px;
  width: 225px;

  display: flex;
  justify-content: center;
  align-items: center;
  background: #696969;
}

.stopwatch-header {
}

.stopwatch-name-input {
  border: none;
  font-size: 20px;
  font-weight: bold;
  width: 150px;
}

.stopwatch-actions {
  display: flex;
  justify-content: space-evenly;
  background: #696969;
}
img {
  background: #696969;
}

.stopwatch-action {
  height: 20px;
  width: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 20px;
  cursor: pointer;
  background: #696969;
}

.stopwatch-action.running {
  color: white;
}

.stopwatch-action:hover {
}

.stopwatch-time.running {
  color: #fff;
  border-bottom: 1px solid white;
}

.stopwatch-time {
  width: 225px;
  font-family: "Gotham Pro";
  font-style: normal;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
  text-align: center;
  color: #9e9e9e;
  padding-bottom: 20px;
  border-bottom: 1px solid #9e9e9e;
  background: #696969;
}

button {
  background-color: #ccc;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
