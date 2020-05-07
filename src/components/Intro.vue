<template>
  <div class="hello">
    <div class="intro">
      <h1>Hi {{ name }}!</h1>
      <h2>
        I'm you personal assistant. I'll help you to find you new perfect city
        for work.
      </h2>
      <h3>So let's start!</h3>
      <h1>Which city do you like the most?</h1>
      <p>
        You can open every city, read information about, compare and choose you
        perfect spot.
      </p>
    </div>
    <div class="cities">
      <div v-for="(city, index) in cities" :key="index">
        <router-link
          :to="{
            name: 'city',
            params: { name: city.name },
          }"
        >
          <div class="city">
            <img
              :src="require(`../assets/${city.wallpaper}`)"
              :alt="city.name"
              class="city-img"
            />
            <div class="centered">{{ city.name.toUpperCase() }}</div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import cities from "../cities";
export default {
  data() {
    return {
      cities: cities,
    };
  },
  name: "Intro",
  props: {
    name: String,
  },
};
</script>

<style scoped>
.cities {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  flex-wrap: wrap;
  margin: 0 5rem;
}
.city {
  position: relative;
  text-align: center;
  font-weight: 900;
  font-size: 20px;
  color: white;
  align-items: center;
  box-sizing: border-box;
  padding: 5px;
  border: 3px solid rgb(81, 102, 100);
}
.centered {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.city-img {
  width: 350px;
  height: 190px;
  filter: brightness(70%);
}
h3 {
  margin: 20px 0 0;
}
a {
  color: #42b983;
}
</style>
