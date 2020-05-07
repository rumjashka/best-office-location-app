<template>
  <div class="city">
    <router-link to="/"><div class="links">BACK</div></router-link>
    <div class="title">
      <h1>Welcome to {{ $route.params.name }}!</h1>
    </div>
    <section v-if="errored">
      <p>Oops... Something went wrong! Please try again later</p>
    </section>
    <div v-else>
      <div class="card">
        <div class="card-header" v-if="currentWeather">
          <h3>CURRENT WEATHER</h3>
          <div class="current-temp">
            <div>
              <span>{{ currentWeather.WeatherText }}</span>
              <div class="temperature">
                {{ currentWeather.Temperature.Metric.Value }}°C
              </div>
            </div>
            <img
              class="weather-icon"
              :src="
                require(`../assets/icons/${currentWeather.WeatherIcon}-s.png`)
              "
            />
            <div>
              <p class="real-feel">
                REAL FEEL
                {{ currentWeather.RealFeelTemperature.Metric.Value }}°C
              </p>
              <br />
              <p class="real-feel">
                REAL FEEL SHADE
                {{ currentWeather.RealFeelTemperatureShade.Metric.Value }}°C
              </p>
            </div>
          </div>
        </div>
      </div>

      <div class="forecast-weather" v-if="forecastWeather">
        <div
          v-for="(forecast, index) in forecastWeather.DailyForecasts"
          :key="index"
        >
          <div class="forecast-card">
            <div class="forecast-header">
              {{ forecast.Date | moment("ddd, MMMM Do") }}
            </div>
            <div class="forecast-content">
              <div class="forecast-content-body">
                <p class="max-min">
                  {{ forecast.Temperature.Maximum.Value }}°C
                </p>
                <img
                  class="weather-icon-forecast"
                  :src="require(`../assets/icons/${forecast.Day.Icon}-s.png`)"
                />
              </div>
              <div class="forecast-content-body">
                <p class="max-min">
                  {{ forecast.Temperature.Minimum.Value }}°C
                </p>
                <img
                  class="weather-icon-forecast"
                  :src="require(`../assets/icons/${forecast.Night.Icon}-s.png`)"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Flights />
  </div>
</template>

<script>
import axios from "axios";
import Flights from "./Flights";
import cities from "../cities";
import { accuweatherApiKey } from "../config";
export default {
  data() {
    return {
      currentWeather: null,
      errored: false,
      forecastWeather: null,
    };
  },
  mounted() {
    let currentCity = cities.find((x) => x.name === this.$route.params.name);
    axios
      .get(
        "http://dataservice.accuweather.com/forecasts/v1/daily/5day/" +
          currentCity.accuweather_id +
          "?apikey=" +
          accuweatherApiKey +
          "&details=true&metric=true"
      )
      .then((response) => (this.forecastWeather = response.data))
      .catch((error) => {
        console.log(error);
        this.errored = true;
      });
    axios
      .get(
        "http://dataservice.accuweather.com/currentconditions/v1/" +
          currentCity.accuweather_id +
          "?apikey=" +
          accuweatherApiKey +
          "&details=true"
      )
      .then((response) => (this.currentWeather = response.data[0]))
      .catch((error) => {
        console.log(error);
        this.errored = true;
      });
  },
  components: {
    Flights,
  },
};
</script>

<style scoped>
.city {
  margin: 0 auto;
}
.links {
  display: flex;
  left: 0;
  font-size: 20px;
  font-weight: 500;
  color: black;
}
.card {
  margin: 30px auto;
  max-width: 40rem;
  height: 18rem;
  background-color: #fff;
  border-radius: 7px;
  box-shadow: 0.3px 0.3px 0.3px 1px rgba(0, 0, 0, 0.2);
  font-family: "Roboto", sans-serif;
}
.card-header {
  padding: 10px;
}
.current-temp {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.weather-icon {
  width: 50%;
}
.weather-icon-forecast {
  width: 100%;
}
.temperature {
  font-size: 40px;
  font-weight: 500;
}
.real-feel {
  margin: 0;
  font-size: 14px;
  font-weight: lighter;
}
.forecast-weather {
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
}
.forecast-card {
  margin: 3rem 0.5rem;
  width: 14rem;
  height: 10rem;
  background-color: #fff;
  border-radius: 7px;
  box-shadow: 0.3px 0.3px 0.3px 1px rgba(0, 0, 0, 0.2);
  font-family: "Roboto", sans-serif;
}
.forecast-header {
  padding: 10px 5px;
  font-size: 24px;
  font-weight: lighter;
}
.forecast-content {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.max-min {
  margin: 5px;
  font-size: 18px;
  font-weight: 600;
}
</style>
