<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp >16 ? 'warm':''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Please input State or City"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
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
      api_key: "e80729efb1fd31224eab91e4b49ff827",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: " ",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}
        &units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let current_date = new Date();
      let months = [
        "Jamuary",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "November",
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[current_date.getDay()];
      let date = current_date.getDate();
      let month = months[current_date.getMonth()];
      let year = current_date.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
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
  background-image: url(./assets/cold-bg.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}
#app.warm {
  background-image: url(./assets/warm-bg.jpg);
}
main {
  min-height: 100vh;
  padding: 26px;
  background-image: linear-gradient(
    to bottom,
    rgb(0, 0, 0, 0.26),
    rgb(0, 0, 0, 0.77)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 16px;
  color: #313131;
  font-size: 21px;

  appearance: none;
  outline: none;
  border: none;
  background: none;

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 15px 0px 15px 0px;
  box-shadow: 0px 0px 10px rgb(0, 0, 0, 0.26);
  transition: 0.5s;
}
.search-box .search-bar:focus {
  border-radius: 0px 15px 0px 15px;
  box-shadow: 0px 0px 30px rgb(0, 0, 0, 0.26);
  background-color: rgba(255, 255, 255, 0.8);
}
.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgb(0, 0, 0, 0.26);
}

.location-box .date {
  color: white;
  font-size: 12px;
  font-weight: 280;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.26);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 15px;
  margin: 30px 0px;

  box-shadow: 4px 8px rgba(67, 107, 126, 0.26);
}

.weather-box .weather {
  color: white;
  font-size: 45px;
  font-weight: 660;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.26);
}
</style>
