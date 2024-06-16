<template>
  <div class="prayer-times">
    <h2>Prayer Times</h2>
    <table>
      <thead>
        <tr>
          <th>Prayer</th>
          <th>Adhan</th>
          <th>Iqamah</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Fajr</td>
          <td class="prayer-time">{{ prayerTimes.fajr }}</td>
          <td class="iqamah-time">{{ calculateIqamahTime(prayerTimes.fajr) }}</td>
        </tr>
        <tr>
          <td>Zuhr</td>
          <td class="prayer-time">{{ prayerTimes.zuhr }}</td>
          <td class="iqamah-time">{{ calculateIqamahTime(prayerTimes.zuhr) }}</td>
        </tr>
        <tr>
          <td>Asr</td>
          <td class="prayer-time">{{ prayerTimes.asr }}</td>
          <td class="iqamah-time">{{ calculateIqamahTime(prayerTimes.asr) }}</td>
        </tr>
        <tr>
          <td>Maghrib</td>
          <td class="prayer-time">{{ prayerTimes.maghrib }}</td>
          <td class="iqamah-time">{{ calculateIqamahTime(prayerTimes.maghrib, 5) }}</td> <!-- Typically 5 mins for Maghrib -->
        </tr>
        <tr>
          <td>Isha</td>
          <td class="prayer-time">{{ prayerTimes.isha }}</td>
          <td class="iqamah-time">{{ calculateIqamahTime(prayerTimes.isha) }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <h3>Next Adhan: {{ prayerTimes.nextAdhan }}</h3>
      <p>Time remaining: {{ countdown }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      prayerTimes: {
        fajr: '',
        zuhr: '',
        asr: '',
        maghrib: '',
        isha: '',
        nextAdhan: ''
      },
      countdown: ''
    };
  },
  created() {
    this.fetchPrayerTimes();
    setInterval(this.updateCountdown, 1000);
  },
  methods: {
    async fetchPrayerTimes() {
      try {
        const response = await axios.get('/api/prayer-times');
        console.log('Fetched prayer times:', response.data);
        this.prayerTimes = response.data;
        this.updateCountdown();
      } catch (error) {
        console.error('Error fetching prayer times:', error);
      }
    },
    updateCountdown() {
      if (!this.prayerTimes.nextAdhan) {
        return;
      }

      const now = new Date();
      const adhanTime = new Date(now);
      const [hours, minutes] = this.prayerTimes.nextAdhan.split(':').map(Number);
      adhanTime.setHours(hours);
      adhanTime.setMinutes(minutes);
      adhanTime.setSeconds(0);

      // If adhanTime is earlier than the current time, set it to the next day
      if (adhanTime < now) {
        adhanTime.setDate(adhanTime.getDate() + 1);
      }

      const timeDiff = adhanTime - now;
      if (timeDiff > 0) {
        const hours = Math.floor(timeDiff / 3600000);
        const minutes = Math.floor((timeDiff % 3600000) / 60000);
        const seconds = Math.floor((timeDiff % 60000) / 1000);
        this.countdown = `${hours}h ${minutes}m ${seconds}s`;
      } else {
        this.countdown = 'Time for Adhan!';
      }
    },
    calculateIqamahTime(adhanTime, offset = 10) {
      if (!adhanTime) return '';
      const [hours, minutes] = adhanTime.split(':').map(Number);
      const iqamahTime = new Date();
      iqamahTime.setHours(hours);
      iqamahTime.setMinutes(minutes + offset);
      return iqamahTime.toTimeString().slice(0, 5);
    }
  }
};
</script>

<style scoped>
.prayer-times {
  text-align: center;
}

table {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  border: 1px solid #ccc;
  font-size: 2vw; /* Responsive font size */
}

.prayer-time, .iqamah-time {
  font-weight: bold;
  color: green;
}

h2, h3 {
  margin: 30px 0;
}

@media (min-width: 1080px) {
  th, td {
    font-size: 1.5vw;
  }
}

@media (max-width: 1080px) and (orientation: portrait) {
  th, td {
    font-size: 3vw;
    padding: 20px;
  }
}

@media (min-width: 1920px) and (orientation: landscape) {
  th, td {
    font-size: 1.25vw;
    padding: 20px;
  }
}
</style>
