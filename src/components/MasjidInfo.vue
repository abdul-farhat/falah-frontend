<template>
    <div class="masjid-info">
      <h2>Masjid Information</h2>
      <p>Name: {{ masjidInfo.name }}</p>
      <p>Address: {{ masjidInfo.address }}</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        masjidInfo: {
          name: '',
          address: ''
        }
      };
    },
    created() {
      this.fetchMasjidInfo();
    },
    methods: {
      async fetchMasjidInfo() {
        try {
          const response = await axios.get('/api/masjid-info');
          console.log('Fetched masjid info:', response.data);
          this.masjidInfo = response.data;
        } catch (error) {
          console.error('Error fetching Masjid information:', error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .masjid-info {
    text-align: center;
    margin-top: 20px;
  }
  </style>
  