<script setup>
import Weather from './components/Weather.vue';
import Mood from './components/Mood.vue';
import Activity from './components/Activity.vue';
import { ref } from 'vue';

const selectedMood = ref('');
const weather = ref('');

console.log('⏳ App.vue - Initial Weather State:', weather.value);

const updateWeather = (description) => {
  weather.value = description === 'NO_WEATHER' ? '' : description;
  console.log(
    `✅ App.vue - Weather Updated: ${weather.value || 'No weather available'}`
  );
};

const updateMood = (mood) => {
  selectedMood.value = mood;
  console.log('✅ App.vue - Mood Updated:', selectedMood.value);
};
</script>

<template>
  <div class="min-h-screen flex flex-col items-center bg-gris text-white px-6">
    <!-- Main Title -->
    <h1 class="text-4xl text-[#E3E8EF] mt-10 mb-2 font-[Poppins] text-center">
      Your Personalized Activity Generator
    </h1>
    <p class="text-gray-400 text-sm text-center mt-2 mb-12 italic">
      Location access helps tailor your activity based on real-time weather.
      Your privacy is respected!
    </p>
    <Weather @updateWeather="updateWeather" />
    <!-- Mood Selection Section  -->
    <div class="w-full max-w-3xl">
      <h2 class="text-2xl text-[#E3E8EF] text-center mb-6">
        How do you feel today?
      </h2>
      <Mood @updateMood="updateMood" />
    </div>
    <!-- Activity Section -->
    <div class="w-full max-w-3xl mt-10">
      <Activity :weather="weather" :mood="selectedMood" />
    </div>
  </div>
</template>
