<template>
  <div>

  </div>
</template>

<script setup>
    import axios from "axios";
    import {useRoute} from "vue-router";

    const route = useRoute()
    const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY
    const getWeatherData = async () => {
      const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${route.query.lat}&lon=${route.query.lon}&appid=${apiKey}&units=imperial`;
      try {
        const weatherData = await axios.get(url);

        // const localOffset = new Date().getTimezoneOffset() * 60000;
        // const utc = weatherData.data.current.dt * 1000 + localOffset;
        // weatherData.data.currentTime =
        //     utc + 1000 * weatherData.data.timezone_offset;
        //
        // // cal hourly weather offset
        // weatherData.data.hourly.forEach((hour) => {
        //   const utc = hour.dt * 1000 + localOffset;
        //   hour.currentTime =
        //       utc + 1000 * weatherData.data.timezone_offset;
        // });

        return weatherData
      } catch (e) {
        console.log(e)
      }
    }
    const weatherData = await getWeatherData()
    console.log(weatherData)
</script>

