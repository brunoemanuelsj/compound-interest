<template>
  <div class="app">
    <input
      type="number"
      v-model="init"
      placeholder="  aporte inicial"
      class="input"
    />
    <input
      type="number"
      v-model="value"
      placeholder="  valor dos aportes/mês"
      class="input"
      id="iptValue"
    />
    <div class="month">
      <input
        type="number"
        v-model="months"
        :placeholder="m1"
        class="monthI"
        id="iptMonths"
      />

      <button class="btnChange" @click="change">
        <svg viewBox="-3 0 30 24">
          <path
            d="M6.99 11L3 15l3.99 4v-3H14v-2H6.99v-3zM21 9l-3.99-4v3H10v2h7.01v3L21 9z"
          ></path>
        </svg>
      </button>
    </div>
    <input
      type="number"
      v-model="tax"
      placeholder="  tax  % a.m."
      class="input"
      id="iptTax"
    />
    <button class="btn" @click="calc">calc</button>
    <h1 class="result">R${{ total }}</h1>

    <div id="percent">
      <div id="perc1"></div>
    </div>

    <div v-if="total" class="finalPresentation">
      <div style="display: flex; flex-direction: row; align-items: center;">
        <div style="width: 10px; height: 10px; background-color: rgb(134, 4, 4); margin-right: 5px;"></div>
        <div>Total investido: R$
        {{ finalInvestedValue }}</div>
      </div>
      <div style="display: flex; flex-direction: row; align-items: center;">
        <div style="width: 10px; height: 10px; background-color: rgb(73, 202, 159); margin-right: 5px;"></div>
        <div>Ganhos juros: R$ {{finalComp}}</div>
      </div>      
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      init: "",
      value: "",
      months: "",
      tax: "",
      total: 0,
      m1: "meses",
      totaltax: 0,
      finalInvestedValue: "",
      finalComp: ""
    };
  },
  methods: {
    calc() {
      document.getElementById("iptValue").style.border = "none";
      document.getElementById("iptMonths").style.border = "none";
      document.getElementById("iptTax").style.border = "none";

      if (!this.value || !this.months || !this.tax) {
        if (!this.value) {
          document.getElementById("iptValue").style.border = "solid 1px red";
        }

        if (!this.months) {
          document.getElementById("iptMonths").style.border = "solid 1px red";
        }

        if (!this.tax) {
          document.getElementById("iptTax").style.border = "solid 1px red";
        }
        return;
      }

      let init;
      let tax = parseFloat(this.tax / 100);
      let months;
      let value = parseFloat(this.value);
      let total = 0;

      if (this.init === "") {
        init = 0;
      } else {
        init = parseFloat(this.init);
      }

      if (this.m1 === "meses") {
        months = parseFloat(this.months);
      } else {
        months = parseFloat(this.months) * 12;
      }
      //this.months = months;
      total = init;
      let totaltax = parseFloat(0);

      for (let i = 0; i < months; i++) {
        totaltax += total * tax;
        total = total + total * tax + value;
      }

      total = total + total * tax;
      totaltax += total * tax;
      //console.log(total + "\n");

      console.log(totaltax);

      this.totaltax = totaltax;

      this.total = total
        .toFixed(2)
        .replace(".", ",")
        .replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1.");

      let widthPercentBar = 300 - (totaltax / total) * 300;
      document.getElementById("perc1").style.width = `${widthPercentBar}px`;

      this.finalInvestedValue = (parseFloat(init) + (parseFloat(months) * parseFloat(value))).toFixed(2).replace(".", ",").replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1.")
      this.finalComp = parseFloat(totaltax).toFixed(2).replace(".", ",").replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1.")
      //console.log(widthPercentBar);
    },
    change() {
      this.m1 === "meses" ? (this.m1 = "anos") : (this.m1 = "meses");
      this.months = "";
    },
  },
};
</script>

<style scoped>
.app {
  width: 100%;
  height: 100%;
  background-color: rgba(255, 246, 233, 0.719);
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: "Noto Sans JP", monospace;
}

.input {
  border: none;
  margin: 35px 0 0 0;
  height: 40px;
  width: 250px;
  color: black;
  font-size: 1em;
  border-radius: 5px;
}

.btn {
  border: none;
  background-color: rgb(235, 110, 52);
  margin-top: 25px;
  height: 40px;
  width: 200px;
  color: white;
  font-size: 1.2em;
  border-radius: 5px;
}

.month {
  margin: 35px 0 0 0;
  height: 40px;
  width: 250px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.monthI {
  border: none;
  color: black;
  font-size: 1em;
  width: 190px;
  padding: 8px;
  box-sizing: border-box;
  border-radius: 5px 0 0 5px;
}

.btnChange {
  border: none;
  background-color: rgb(235, 110, 52);
  height: 40px;
  width: 60px;
  color: white;
  font-size: 1em;
  border-radius: 0px 5px 5px 0;
}

.btnChange:active,
.btn:active {
  background-color: rgb(109, 57, 34);
}

#percent {
  height: 20px;
  width: 300px;
  border: solid 1px black;
  display: flex;
  flex-direction: row;
  background-color: rgb(73, 202, 159);
}

#perc1 {
  width: 300px;
  height: 20px;
  background-color: rgb(134, 4, 4);
}

.finalPresentation{
  margin: 20px 0 0 0;
  font-size: 1.2em;
}
</style>