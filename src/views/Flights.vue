<template>
  <div class="about">
    <div class="subtitle">
      Select dates and I will show you the cheapest flights from
      {{ $route.params.name }}
    </div>
    <div class="calendar">
      <date-picker
        v-model="time"
        placeholder="Select datetime range"
        range
        @pick="handlePick"
      ></date-picker>
    </div>
    <div v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this information at the moment,
        please try back later
      </p>
    </div>
    <div class="card-holder" v-else>
      <Spinner v-if="loading" />
      <div v-else v-for="(city, index) in info" :key="index">
        <div class="card">
          <div class="card-layout">
            <div>
              City: <b>{{ city.cityTo }}</b>
            </div>
            <div>
              Price: <b>€{{ city.price }}</b>
            </div>
            <div>
              Date:<b>{{ city.aTime | moment("ddd, MMMM Do") }}</b>
            </div>
          </div>
          <div class="divider"></div>
          <div class="card-layout-content">
            <div>
              Duration: <b>{{ city.fly_duration }} </b>
            </div>
            <div class="flights-time">
              <div>
                {{ city.aTime | moment("k:mm") }} {{ city.cityFrom }}
                {{ city.cityCodeFrom }}
              </div>
              <div>
                {{ city.dTime | moment("k:mm") }}
                {{ city.cityTo }}
                {{ city.cityCodeTo }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";
import Spinner from "../components/Spinner";
import "vue2-datepicker/index.css";
import axios from "axios";
import cities from "../cities";

export default {
  components: { DatePicker, Spinner },
  data() {
    return {
      info: null,
      loading: false,
      errored: false,
      time: [],
    };
  },
  methods: {
    handlePick() {
      let currentCity = cities.find((x) => x.name === this.$route.params.name);
      if (this.time.length < 2) {
        return;
      }
      this.loading = true;
      var dateFrom = this.$moment(this.time[0]).format("DD/MM/YYYY");
      var dateTo = this.$moment(this.time[1]).format("DD/MM/YYYY");
      axios
        .get(
          "https://api.skypicker.com/flights?flyFrom=" +
            currentCity.kiwi_id +
            "&dateFrom=" +
            dateFrom +
            "&dateTo=" +
            dateTo +
            "&partner=picky&v=3&one_for_city=1"
        )
        .then(
          function(response) {
            this.info = response.data.data;
          }.bind(this)
        )
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<style scoped>
.about {
  margin-bottom: 8rem;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}
.subtitle {
  font-size: 25px;
  font-weight: 600;
}
.calendar {
  margin: 3rem auto 0.5rem auto;
}
.card {
  margin: 5px auto;
  padding: 30px;
  max-width: 40rem;
  background-color: #fff;
  border-radius: 7px;
  box-shadow: 0.3px 0.3px 0.3px 1px rgba(0, 0, 0, 0.2);
  font-family: "Roboto", sans-serif;
}
.card-layout {
  padding-bottom: 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.card-holder {
  display: flex;
  flex-direction: column;
}
.divider {
  flex-grow: 1;
  border-bottom: 1px solid rgba(0, 0, 0, 0.082);
  margin: 5px;
}
.card-layout-content {
  height: 1rem;
  padding: 20px;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}
.flights-time {
  text-align: left;
}
</style>
