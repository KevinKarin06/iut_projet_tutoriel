<template>
  <div class="main">
    <div class="vld-parent">
      <loading
        :active="isLoading"
        :can-cancel="true"
        :on-cancel="onCancel"
        :is-full-page="fullPage"
      />
    </div>
    <div v-if="!isLoading">
      <div id="content">
        <card :callback="handleCardClick" :data="active" />
        <card :callback="handleCardClick" :data="confirm" />
        <card :callback="handleCardClick" :data="cured" />
        <card :callback="handleCardClick" :data="death" />
      </div>
      <div class="chart-map">
        <div id="chart">
          <line-chart
            :width="500"
            :height="300"
            :labels="labels"
            :datasets="testData"
            :options="$options.options"
          ></line-chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
import axios from "axios";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";
//import CovidChart from "./CovidChart.vue";
//import MonthlySalesChart from "./MonthlySalesChart.vue";
import LineChart from "./LineChart";
import numeral from "numeral";
import moment from 'moment';

const options = {
  elements: {
    point: {
      radius: 0,
    },
  },
  scales: {
    yAxes: [
      {
        ticks: {
          beginAtZero: true,
          callback: (value) => numeral(value).format("0,0"),
        },
      },
    ],
  },
  tooltips: {
    mode: "index",
    callbacks: {
      label(tooltipItem, data) {
        const label = data.datasets[tooltipItem.datasetIndex].label;
        const value = numeral(tooltipItem.yLabel).format("0,0");

        return `${label}: ${value}`;
      },
    },
  },
};
export default {
  
  options,
  data: function () {
    return {
      data: [],
      isLoading: true,
      dataset: [
        {
          label: "Active",
          borderColor: "rgba(127, 189, 255, 0.5)",
          backgroundColor: "rgba(127, 189, 255, 0.1)",
          data: [],
        },
        {
          label: "Confirmed",
          borderColor: "rgba(255, 131, 156, 0.5)",
          backgroundColor: "rgba(255, 131, 156, 0.1)",
          data: [],
        },
        {
          label: "Recovered",
          borderColor: "rgba(132, 225, 154, 0.5)",
          backgroundColor: "rgba(132, 225, 154, 0.1)",
          data: [],
        },
        {
          label: "Deaths",
          borderColor: "rgba(180, 186, 190, 0.5)",
          backgroundColor: "rgba(180, 186, 190, 0.1)",
          data: [],
        },
      ],
    };
  },
  components: { Card, LineChart, Loading },
  name: "MainComponent",
  created() {},
  methods: {
    testDat: function () {
      let temp = this.data.map((el) => el.Active);
      this.dataset[0].data = temp;
      return this.dataset;
    },
    handleCardClick: function (id) {},
    growth: function (arr) {
      let total = arr[arr.length - 1];
      let week = arr[arr.length - 7];
      let growth = total - week;
      return growth;
    },
  },
  computed: {
    labels: function () {
      return this.data.map((el) => moment(el.Date.split()[0]).format("MMM Do YYYY"));
    },
    testData: function () {
      let active = this.data.map((el) => el.Active);
      let confirmed = this.data.map((el) => el.Confirmed);
      let recovered = this.data.map((el) => el.Recovered);
      let deaths = this.data.map((el) => el.Deaths);
      this.dataset[0].data = active;
      this.dataset[1].data = confirmed;
      this.dataset[2].data = recovered;
      this.dataset[3].data = deaths;
      return this.dataset;
    },
    testData1: function (num) {
      return num;
    },
    active: function () {
      let active = this.data.map((el) => {
        return el.Active;
      });
      let length = active.length;
      let total = active[length - 1];
      let growth = this.growth(active);

      return {
        id: 0,
        title: "Active",
        growth: growth,
        growthType: "positive",
        total: total,
        color: "#7fbdff",
      };
    },
    cured: function () {
      let cured = this.data.map((el) => {
        return el.Recovered;
      });
      let length = cured.length;
      let total = cured[length - 1];
      let growth = this.growth(cured);
      return {
        id: 1,
        title: "Recovered",
        growth: growth,
        growthType: "positive",
        total: total,
        color: "#84e19a",
      };
    },
    death: function () {
      let death = this.data.map((el) => {
        return el.Deaths;
      });
      let length = death.length;
      let total = death[length - 1];
      let growth = this.growth(death);
      return {
        id: 2,
        title: "Death",
        growth: growth,
        growthType: "positive",
        total: total,
        color: "#b4babe",
      };
    },
    confirm: function () {
      let confirmed = this.data.map((el) => {
        return el.Confirmed;
      });
      let length = confirmed.length;
      let total = confirmed[length - 1];
      let growth = this.growth(confirmed);
      return {
        id: 3,
        title: "Confirmed",
        growth: growth,
        growthType: "positive",
        total: total,
        color: "#ff839c",
      };
    },
  },
  mounted() {
    console.log("main component mounted");
    axios
      .get("https://api.covid19api.com/dayone/country/cameroon", {
        params: {},
      })
      .then((response) => [
        (this.data = response.data),
        (this.isLoading = false),
        this.testData("Active"),
      ])
      .catch((error) => {
        console.log(error);
        this.isLoading = false;
      });
  },
};
</script>

<style scoped>
/* .main {
  width: 100%;
  height: 100%;
  background: red;
} */
#content {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}
.chart-map {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  height: 50vh;
}
#map,
#chart {
  height: 50vh;
  width: 100%;
  padding: 10px;
}
#map {
  /* background-image: url("../assets/cm-02.svg"); */
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
#map div::after {
  content: "";
  height: 5px;
  width: 5px;
  border-radius: 50%;
  background-color: brown;
}
/* .dot{
  height: 5px;
  width: 5px;
  border-radius: 50%;
  background-color: brown;
} */
</style>
