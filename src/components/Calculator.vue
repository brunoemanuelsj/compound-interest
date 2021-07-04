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
    />
    <div class="month">
      <input type="number" v-model="months" :placeholder="m1" class="monthI" />

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
    />
    <button class="btn" @click="calc">calc</button>
    <h1 class="result">R${{ total }}</h1>
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
      m1: "meses"
    };
  },
  methods: {
    calc() {
      if (!this.value || !this.months || !this.tax) {
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

      //console.log(init + "\n");
      //console.log(tax + "\n");
      //console.log(months + "\n");
      //console.log(value + "\n");

      total = init;

      for (let i = 1; i < months; i++) {
        total = total + total * tax + value;
      }

      total = total + total * tax;
      //console.log(total + "\n");

      this.total = total.toFixed(2).replace('.',',').replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1.")

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
</style>