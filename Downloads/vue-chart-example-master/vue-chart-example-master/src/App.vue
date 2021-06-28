<template>
  <div class="app container has-text-centered">
    <div class="banner">Covid19Cameroon</div>
    <main-component />
    <!-- <test-component /> -->
  </div>
</template>

<script>
import MonthlySalesChart from "./components/MonthlySalesChart";
import MainComponent from "./components/MainComponent";
import TestComponent from "./components/TestComponent.vue";
export default {
  name: "app",
  components: {
    MonthlySalesChart,
    MainComponent,
    TestComponent
  },
};
</script>
<style>
#app {
  font-family: "Poppins", Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
  width: 80vw;
  margin: 0px auto;
}
.banner {
  margin: 10px 0px;
  padding: 10px;
  font-size: 1.5rem;
  background: red;
}
</style>