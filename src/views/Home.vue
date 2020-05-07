<template>
  <div class="home">
    <div class="background-image"></div>
    <Intro class="block" name="Jamie" />
  </div>
</template>

<script>
import Intro from "@/components/Intro.vue";

export default {
  name: "Home",
  components: {
    Intro,
  },
};
</script>

<style scoped>
.home {
  color: #eeee;
}
.background-image {
  position: fixed;
  left: 0;
  right: 0;
  z-index: 1;
  display: block;
  height: 100vh;
  background-size: cover;
  background-image: url("../assets/bakground-img.jpg");
  filter: brightness(50%);
}
.block {
  position: fixed;
  overflow-y: scroll;
  background-color: rgba(181, 238, 230, 0.322);
  padding-top: 5rem;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 1;
}
</style>
