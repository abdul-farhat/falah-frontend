<template>
  <div class="clock">
    <div class="hour" :style="hourStyle"></div>
    <div class="minute" :style="minuteStyle"></div>
    <div class="second" :style="secondStyle"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      now: new Date()
    };
  },
  computed: {
    hourStyle() {
      const hours = this.now.getHours() % 12;
      const minutes = this.now.getMinutes();
      const hourDeg = (hours * 30) + (minutes * 0.5); // 360deg / 12hours = 30deg/hour
      return { transform: `rotate(${hourDeg}deg)` };
    },
    minuteStyle() {
      const minutes = this.now.getMinutes();
      const minuteDeg = (minutes * 6); // 360deg / 60minutes = 6deg/minute
      return { transform: `rotate(${minuteDeg}deg)` };
    },
    secondStyle() {
      const seconds = this.now.getSeconds();
      const secondDeg = (seconds * 6); // 360deg / 60seconds = 6deg/second
      return { transform: `rotate(${secondDeg}deg)` };
    }
  },
  mounted() {
    setInterval(() => {
      this.now = new Date();
    }, 1000);
  }
};
</script>

<style scoped>
.clock {
  width: 300px;
  height: 300px;
  border: 5px solid #2c3e50;
  border-radius: 50%;
  position: relative;
  margin: 20px 0;
}

.hour, .minute, .second {
  position: absolute;
  width: 50%;
  height: 2px;
  background: #2c3e50;
  top: 50%;
  transform-origin: 100%;
}

.hour {
  height: 4px;
}

.second {
  background: green; /* Change the color of the second hand to green */
  height: 1px;
}

@media (min-width: 1080px) {
  .clock {
    width: 400px;
    height: 400px;
  }
}
</style>
