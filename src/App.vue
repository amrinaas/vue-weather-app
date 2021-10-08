<template>
  <div id="App" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 
    'rain' : '' ">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          v-on:keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '2af1fff566028c766447b583f4454abe',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let dates = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      let day = days[dates.getDay()];
      let date = dates.getDate();
      let month = months[dates.getMonth()];
      let year = dates.getFullYear();

      return `${day}, ${date} ${month} ${year}`
    } 
  }
}
</script>

<style>
 * {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
 }

 body {
   font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
 }

 #App {
   background-image: url('./assets/rain.webp');
   background-size: cover;
   background-position: bottom;
   transition: 0.4s;
 }

 #App.rain {
   background-image: url('./assets/cloud.png');
 }

 main {
   min-height: 100vh;
   padding: 25px;
   background-image: linear-gradient(to bottom rgba(0. 0, 0, 0.25), rgba(0, 0, 0, 0.75));
 }

 .search-box {
   width: 100%;
   margin-bottom: 30px;
 }

 .search-box .search-bar {
   display: block;
   width: 100%;
   padding: 15px;

   color: #313131;
   font-size: 20px;

   appearance: none;
   border: none;
   outline: none;
   background: none;

   box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
   background-color: rgba(255, 255, 255, 0.5);
   border-radius: 0px 16px 0px 16px;
   transition: 0.4s;
 }

 .search-box .search-bar:focus {
   box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
   background-color: rgba(225, 225, 225, 0.75);
   border-radius: 16px 0px 16px 0px;
 }

 .location-box .location {
   color: #fff;
   font-size: 32px;
   font-weight: 500;
   text-align: center;
   text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
 }

 .location-box .date {
   color: #fff;
   font-size: 32px;
   font-weight: 500;
   text-align: center;
   font-style: italic;
   text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
 }

 .weather-box {
   text-align: center;
 }

 .weather-box .temp {
   display: inline-block;
   padding: 10px 25px;
   columns: #fff;
   font-weight: 900;
   font-size: 102px;

   text-shadow: px 6px rgba(0, 0, 0, 0.25);
   background-color: rgba(255, 255, 255, 0.25);
   border-radius: 16px;
   margin: 30px 0;

   box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
 }

  .weather-box .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

</style>
