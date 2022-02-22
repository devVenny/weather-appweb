<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          @keypress="fetchWeather"
          v-model="cityName"
        />
      </div>

      <section class="weather-wrap" v-if="isShown === true">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">
            {{ dateBuilder() }}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}<span class="celcius">℃</span>
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      API_KEY: "091dae33e14b9ff632dfadae805d202e",
      baseUrl: "https://api.openweathermap.org/data/2.5/",
      cityName: "",
      weather: {},
      isShown: false,
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        const fetchURL = `${this.baseUrl}weather?q=${this.cityName}&appid=${this.API_KEY}&units=metric`;
        fetch(fetchURL)
          .then((res) => {
            return res.json();
          })
          .then((res) => {
            return this.setWeather(res);
          })
          .catch((err) => console.log(err));
      }
    },
    setWeather(data) {
      console.log(data);
      if (data) {
        this.isShown = true;
        this.weather = data;
      }
    },
    dateBuilder() {
      const d = new Date();
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
      const year = d.getFullYear();
      const month = months[d.getMonth()];
      const day = days[d.getDay()];
      const date = d.getDate();
      const today = `${day} ${date} ${month} ${year}`;
      return today;
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
  background-image: url("./assets/cold-bg.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
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

.celcius {
  font-size: 38px;
}
</style>
