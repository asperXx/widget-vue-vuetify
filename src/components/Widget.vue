<template>
  <v-app>
    <v-card class="widget">
      <div class="header__title">Курс {{ currency }} сегодня</div>
      <div class="container">
        <v-tabs
          background-color="#FFE782"
          show-arrows
          color="#2B2D33"
          v-model="model"
        >
          <v-tabs-slider color="white"></v-tabs-slider>
          <v-tab
            v-for="(card, id) in rates"
            :key="id"
            :href="'#' + id"
            active-class="white tab__active"
            @change="currency = id"
            @click="
              setCurrency();
              overlay = !overlay;
            "
            class="tabs"
            >{{ id }}</v-tab
          >
        </v-tabs>
        <div class="inputValueContainer">
          <v-text-field
            color="#D9D9D9"
            reverse
            v-model="value"
            class="inputValue"
          ></v-text-field>
          <span class="mt-6 ml-2 currency__span">{{ currency }}</span>
        </div>
        <v-tabs-items v-model="model" class="container__cards">
          <v-tab-item v-for="(card, id) in rates" :key="id" :value="id">
            <v-row class="pl-4 pr-4">
              <v-col
                cols="12"
                xs="12"
                md="6"
                v-for="(card, id, index) in currencyValues"
                :key="id"
                v-show="showCards(index)"
              >
                <v-card round class="course_card mb-0">
                  <div>
                    <span class="value_currency__span">{{ value }}</span>
                    <span class="currency__span"> {{ currency }} = </span>
                  </div>
                  <div>
                    <span class="calc_currency__span">
                      {{ (value * card).toFixed(2) }} </span
                    ><span class="value_currency__span">{{ id }}</span>
                  </div>
                </v-card>
              </v-col>
            </v-row>
          </v-tab-item>
        </v-tabs-items>
        <div class="paginator">
          <v-btn color="white" :disabled="idx == 0" @click="idx -= 4">
            <v-icon left>mdi-chevron-left</v-icon>Назад
          </v-btn>
          <v-btn color="white" :disabled="idx == 28" @click="idx += 4">
            Далее
            <v-icon right>mdi-chevron-right</v-icon>
          </v-btn>
        </div>
      </div>
      <v-overlay :value="overlay">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>
    </v-card>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      rates: null,
      model: "",
      currency: "CAD",
      currencyValues: {},
      value: 1,
      idx: 0,
      overlay: false,
      widthWindow: 1024,
    };
  },
  created() {
    window.addEventListener("resize", this.updateWidth);
  },
  mounted() {
    axios
      .get("https://api.exchangeratesapi.io/latest")
      .then((response) => (this.rates = response.data.rates));

    axios
      .get("https://api.exchangeratesapi.io/latest?base=" + this.currency)
      .then((response) => (this.currencyValues = response.data.rates));
  },
  methods: {
    setCurrency() {
      axios
        .get("https://api.exchangeratesapi.io/latest?base=" + this.currency)
        .then((response) => (this.currencyValues = response.data.rates));
    },
    updateWidth() {
      this.widthWindow = window.innerWidth;
    },
    showCards(index) {
      if (this.widthWindow > 600) {
        if (this.idx <= index && this.idx >= index - 3) {
          return true;
        }
      } else if (this.widthWindow < 600) {
        if (this.idx <= index && this.idx >= index - 1) {
          return true;
        }
      }
    },
  },
  watch: {
    overlay(val) {
      val &&
        setTimeout(() => {
          this.overlay = false;
        }, 1000);
    },
  },
};
</script>

<style scoped lang="scss">
.widget {
  max-height: 582px;
  min-width: 320px;
  @media only screen and (min-width: 320px) {
    max-width: 320px;
  }
  @media only screen and (min-width: 480px) {
    max-width: 480px;
  }
  @media only screen and (min-width: 720px) {
    max-width: 720px;
  }
  @media only screen and (min-width: 1024px) {
    max-width: 1024px;
  }
}
.tabs {
  font-weight: 600;
  color: #ccae68;
}
.tab__active {
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.header__title {
  background-color: #ffe782;
  height: 63px;
  font-size: 21px;
  padding: 30px 0px 0px 25px;
  color: #2b2d33;
  width: 100%;
}

.inputValueContainer {
  display: flex;
  width: 160px;
  margin: 5px 20px 0 0;
  align-self: flex-end;
}
.course_card {
  display: flex;
  flex-direction: column;
  height: 138px;
  padding: 10px 25px;
  justify-content: space-between;
}

.paginator {
  display: flex;
  align-self: center;
  margin: 30px 0 24px 0;
  width: 242px;
  justify-content: space-between;
}
.currency__span {
  font-weight: 300;
  color: #b9b9b9;
  font-size: 18pt;
  letter-spacing: 0.02;
}
.value_currency__span {
  font-weight: 300;
  color: #2b2d33;
  font-size: 18pt;
  letter-spacing: 0.02;
}
.container {
  display: flex;
  flex-direction: column;
  padding: 0;
  &__cards {
    margin-top: -10px;
  }
}
.calc_currency__span {
  font-weight: 300;
  color: #2b2d33;
  font-size: 44pt;
  letter-spacing: 0.02;
}
</style>
