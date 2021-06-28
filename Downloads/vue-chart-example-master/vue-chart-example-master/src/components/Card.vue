<template>
  <div v-bind:style="testStyles" v-on:click="test" class="card">
    <h4>{{ data.title }}</h4>
    <p>
      {{
        data.growthType == "positive"
          ? "+" + numeral(data.growth).format()
          : numeral(data.growth).format()
      }}
    </p>
    <p id="total">{{ numeral(data.total).format("0,0") }}</p>
    <div v-if="data.growthType == 'positive'" class="line">
      <div class="chart-line">
        <img src="../assets/line_up.png" alt="icon" />
      </div>
    </div>
    <div v-else>
      <div class="chart-line">
        <img src="../assets/line_down.png" alt="icon" />
      </div>
    </div>
  </div>
</template>


<script>
var numeral = require("numeral");
export default {
  data() {
    return {
      numeral: numeral,
      cardTranslate: -500,
      testStyles: {
        "background-color": this.data.color,
        transform: `translateY(${this.cardTranslate}px)`,
      },
    };
  },
  props: ["data", "callback"],
  mounted() {
    this.cardTranslate = -500;
    console.log(this.cardTranslate);
  },
  methods: {
    test: function () {
      this.callback(this.data.id);
    },
  },
};
</script>


<style scoped>
.card {
  height: 250px;
  width: 200px;
  display: flex;
  flex-direction: column;
  padding: 8px;
  justify-content: space-evenly;
  align-items: center;
  transition: 2s all ease;
  /* transform: translateY(0px); */
}
.card:hover {
  background-color: rgb(41, 165, 202);
  cursor: pointer;
}
.chart-line {
  height: 65px;
  width: 65px;
}
h4 {
  font-size: 1.2rem;
  font-weight: bold;
}
#total{
  font-size: 1.2rem;
  letter-spacing: 1.2px;
}
.chart-line > img{
 margin-left: -20px;
}
.chart-line{
   margin-bottom: 10px;
}
</style>