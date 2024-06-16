<template>
  <div class="current-time">
    <AnalogueClock />
    <div>{{ digitalTime }}</div>
    <div>{{ date }}</div>
  </div>
</template>

<script>
import AnalogueClock from './AnalogueClock.vue';

export default {
  components: {
    AnalogueClock
  },
  data() {
    return {
      digitalTime: '',
      date: ''
    };
  },
  created() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
  },
  methods: {
    updateTime() {
      const now = new Date();
      this.digitalTime = now.toLocaleTimeString();
      this.date = now.toLocaleDateString('en-GB', {
        weekday: 'long',
        day: '2-digit',
        month: 'long',
        year: 'numeric'
      });
    }
  }
};
</script>

<style scoped>
.current-time {
  text-align: center;
}
.clock {
  margin-top: 20px;
}
</style>
