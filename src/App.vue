<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="weather-wrap-empty" v-else>
        <p class="empty-msg">Search your city above</p>
      </div>
    </main>
  </div>
</template>


<script>
import { API_KEY, URL_BASE } from "../apikey.js"

export default {
  name: 'app',
  data() {
    return {
      api_key: API_KEY,
      url_base: URL_BASE,
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`).then(res => {
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults(results) {
      this.weather = results
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thrusday", "Friday", "Saturday"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
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
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');

}

main {
  min-height: 100vh;
  padding: 24px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 32px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 16px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
  background: none;
  background-color: rgba(255, 255, 255, 0.50);
  outline: none;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
  box-shadow: 0 20px 20px rgba(0, 0, 0, 0.15);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-wrap-empty {
  min-height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;

}

.empty-msg {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  justify-content: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 12px 24px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.30);
  border-radius: 16px;
  margin: 32px 0;
  box-shadow: 3px 6px 0 rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px 0 rgba(0, 0, 0, 0.25);

}
</style>
