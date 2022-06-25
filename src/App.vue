<template>
  <div
    id="app"
    :class="
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Clouds') ? 'clouds' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Clear') ? 'clear' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Rain') ? 'rain' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Drizzle') ? 'drizzle' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Snow') ? 'snow' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Atmosphere') ? 'atmosphere' : 
      (typeof weather.main != 'undefined' && weather.weather[0].main === 'Thunderstorm') ? 'thunderstorm' : ''"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <button @click="showExtra()">  {{ this.show ? 'Hide Extra Info' : 'Show Extra Info' }} </button>
          <div v-if="show" class="extras">
            <div>Minimum Temp : {{ Math.round(weather.main.temp_min) }}</div>
            <div>Maximum Temp :{{ Math.round(weather.main.temp_max) }}</div>
            <div v-if="weather.rain">1 Hour Rain Expectation : {{ weather.rain["1h"] * 100 }}% </div>
            <div> Humidity : {{ weather.main.humidity }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "", // Place your API key in between quotation marks
      query: "Landau",
      weather: {},
      show: false
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather);
    },
    showExtra(){
      this.show = !this.show;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/home.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.clouds {
  background-image: url("./assets/clouds.jpg");
}
<!-- #app.atmosphere {
  background-image: url("./assets/atmosphere.jpg");
} -->
#app.clear {
  background-image: url("./assets/clear.jpg");
}
#app.drizzle {
  background-image: url("./assets/drizzle.jpg");
}
#app.rain {
  background-image: url("./assets/rain.jpg");
}
#app.snow {
  background-image: url("./assets/snow.jpg");
}
<!-- #app.thunderstorm {
  background-image: url("./assets/thunderstorm.jpg");
} -->
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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
  background-color: rgba(255, 255, 255, 0.75);
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
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .extras {
  color: #fff;
  font-size: 28px;
  font-weight: 250;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
button {
  text-align: center;
  padding: 10px 25px;
  box-shadow: 3px 6px rgba(0.4, 0.1, 0.03, 0.15);
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.35);
  margin: 20px 5px;
  border-color: rgba(255, 255, 255, 0.15);
}
</style>
