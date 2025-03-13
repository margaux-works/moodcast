<script setup>
import { onMounted, ref } from 'vue';

const emit = defineEmits(['updateWeather']);
const weather = ref('');

onMounted(() => {
  console.log('⏳ Weather.vue - Fetching weather data...');

  if (!navigator.geolocation) {
    console.error('Geolocation is not supported by your browser');
    return;
  }

  navigator.geolocation.getCurrentPosition(
    async (position) => {
      console.log(
        '✅ Weather.vue - Got user location:',
        position.coords.latitude,
        position.coords.longitude
      );

      let lat = position.coords.latitude;
      let lon = position.coords.longitude;

      const apiKey = import.meta.env.VITE_WEATHER_API_KEY;
      const apiURL = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&exclude={part}&appid=${apiKey}`;

      try {
        const response = await fetch(apiURL);
        if (!response.ok) throw new Error('Failed to fetch weather data');
        const data = await response.json();

        const weatherDescription =
          data.weather?.[0]?.description || 'NO_WEATHER';
        console.log(
          '✅ Weather.vue - Weather Data Fetched:',
          weatherDescription
        );

        emit('updateWeather', weatherDescription); //  Emit to App.vue
      } catch (error) {
        console.error('❌ Weather.vue - Error fetching weather:', error);
        emit('updateWeather', 'NO_WEATHER'); // Notify parent that weather is unavailable
      }
    },
    (err) => {
      console.error('❌ Weather.vue - Error getting geolocation:', err.message);
      emit('updateWeather', 'NO_WEATHER'); // Notify parent that location access was denied
    }
  );
});
</script>

<template>
  <div id="weather-output"></div>
</template>

<style></style>
