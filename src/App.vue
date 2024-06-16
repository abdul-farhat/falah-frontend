<template>
  <div id="app">
    <div class="content">
      <div class="logo-clock-container">
        <img alt="Vue logo" src="./assets/logo.png" class="logo">
        <CurrentTime />
      </div>
      <PrayerTimes />
    </div>
  </div>
</template>

<script>
import CurrentTime from './components/CurrentTime.vue';
import PrayerTimes from './components/PrayerTimes.vue';

export default {
  name: 'App',
  components: {
    CurrentTime,
    PrayerTimes
  }
};
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  background: #f4f4f4;
  color: #2c3e50;
}

#app {
  display: flex;
  height: 100vh;
  text-align: center;
  flex-direction: column; /* Default to column for portrait mode */
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  text-align: center;
}

.logo-clock-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.logo {
  width: 200px;
  margin-bottom: 20px;
}

@media (min-width: 1080px) {
  .logo {
    width: 250px;
  }
}

@media (min-width: 1920px) and (orientation: landscape) {
  #app {
    flex-direction: row;
  }

  .content {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 100vh;
  }

  .logo-clock-container {
    margin-bottom: 0;
    align-items: center;
    margin-left: 5%; /* Adjust margin to bring closer to edge */
    margin-right: 5%; /* Ensure proper spacing from table */
  }

  table {
    max-width: 100%;
    font-size: 2.5em; /* Adjust font size */
  }

  th, td {
    font-size: 2.5em; /* Adjust font size */
    padding: 25px; /* Adjust padding */
  }
}

@media (max-width: 1080px) and (orientation: portrait) {
  .content {
    flex-direction: column;
  }

  .logo-clock-container {
    margin-bottom: 40px;
  }

  table {
    max-width: 100%;
  }

  th, td {
    font-size: 3em; /* 1.5x font size */
    padding: 30px; /* 1.5x padding */
  }
}
</style>
