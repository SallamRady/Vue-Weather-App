<template>
  <main
    :class="{
      Clouds:
        typeof weather.main != 'undefined' &&
        weather.weather[0].main == 'Clouds',
      Clear:
        typeof weather.main != 'undefined' &&
        weather.weather[0].main == 'Clear',
      Sunny:
        typeof weather.main != 'undefined' &&
        weather.weather[0].main == 'Sunny',
    }"
  >
    <h1>Weather App</h1>
    <div class="search-box">
      <input
        type="text"
        class="search-bar"
        v-model="query"
        @keypress="fetchWeather"
        placeholder="Searching..."
      />
    </div>
    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="date">{{ dateBuilder }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "186751a2af9086920b0d67f9ceecafdb",
      api_base: "https://api.openweathermap.org/data/2.5/weather?q=",
      query: "",
      weather: {},
    };
  },
  computed: {
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
  methods: {
    fetchWeather(event) {
      let url =
        this.api_base + this.query + "&units=metric&APPID=" + this.api_key;
      if (event.key == "Enter") {
        fetch(url)
          .then((response) => response.json())
          .then((data) => {
            this.weather = data;
            console.log("data : ", data);
          })
          .catch((err) => console.log("error : ", err));
      }
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
main {
  background-image: url("./assets/Warm.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  min-height: 100vh;
  padding: 1.5rem;
  background-image: linear-gradient(
    bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
main.Clouds {
  background-image: url("./assets/Clouds.jpg");
}
main.Sunny {
  background-image: url("./assets/Sunny.jpg");
}
main.Clear {
  background-image: url("./assets/Clear.jpg");
}

main h1 {
  text-align: center;
  margin: 2rem auto;
  font-size: 3rem;
  color: #fff;
  font-weight: bolder;
  text-shadow: 1px 2px 3px;
}
.search-box {
  width: 100%;
  margin-bottom: 2rem;
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
</style>
