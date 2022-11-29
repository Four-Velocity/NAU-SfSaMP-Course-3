<template class="main">
  <v-toolbar color="primary">
    <v-toolbar-title> Лабораторна робота №3 </v-toolbar-title>
  </v-toolbar>

  <div id="t4-main">
    <div class="result">{{ calculator.showValue }}</div>
    <div class="buttons">
      <v-btn class="operator" id="rst" @click="calculator.clean_up(0)">AC</v-btn>
      <v-btn class="operator secondary" id="pm" variant="tonal" :disabled="true">±</v-btn>
      <v-btn class="operator secondary" id="percent" variant="tonal" :disabled="true">%</v-btn>
      <v-btn class="operator" id="div" @click="calculator.setAction('div')">÷</v-btn>
      <v-btn class="digit" id="seven" variant="outlined" @click="calculator.numberPressed(7)">7</v-btn>
      <v-btn class="digit" id="eight" variant="outlined" @click="calculator.numberPressed(8)">8</v-btn>
      <v-btn class="digit" id="nine" variant="outlined" @click="calculator.numberPressed(9)">9</v-btn>
      <v-btn class="operator" id="mult" @click="calculator.setAction('mult')">✕</v-btn>
      <v-btn class="digit" id="four" variant="outlined" @click="calculator.numberPressed(4)">4</v-btn>
      <v-btn class="digit" id="five" variant="outlined" @click="calculator.numberPressed(5)">5</v-btn>
      <v-btn class="digit" id="six" variant="outlined" @click="calculator.numberPressed(6)">6</v-btn>
      <v-btn class="operator" id="minus" @click="calculator.setAction('minus')">-</v-btn>
      <v-btn class="digit" id="one" variant="outlined" @click="calculator.numberPressed(1)">1</v-btn>
      <v-btn class="digit" id="two" variant="outlined" @click="calculator.numberPressed(2)">2</v-btn>
      <v-btn class="digit" id="three" variant="outlined" @click="calculator.numberPressed(3)">3</v-btn>
      <v-btn class="operator" id="plus" @click="calculator.setAction('plus')">+</v-btn>
      <v-btn class="digit" id="zero" variant="outlined" @click="calculator.numberPressed(0)">0</v-btn>
      <v-btn class="operator secondary" id="dot" variant="tonal" :disabled="true">.</v-btn>
      <v-btn class="operator" id="equal" @click="calculator.calculate()">=</v-btn>
    </div>
  </div>
</template>
  
<style scoped>
#t4-main {
  flex: 0 1 60%;
  min-width: 90vw;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
  margin-top: 25px;
}

.buttons,
.result {
  display: flex;
  flex: 0 1 100%;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.buttons {
  /* gap: 5px; */
  justify-content: center;
}

.result {
  height: 50px;
  border-radius: 15px;
  background: black;
  color: white;
  font-size: 1.5em;
  justify-content: flex-end;
  align-items: center;
  padding: 0 25px;
}

button {
  display: flex;
  flex: 0 1 25%;
  justify-content: center;
  align-items: center;
  height: 50px;
  font-size: 2em;
}

#zero {
  flex-grow: 2;
}

.secondary {
  border: 1px solid;
}
</style>

<script>
class Calculator {
  constructor() {
    this.first = 0;
    this.second = NaN;
    this.action = null;
  }

  clean_up(first = 0) {
    this.first = first;
    this.second = NaN;
    this.action = null;
  }

  get showValue() {
    const symbols = {
      plus: " +",
      minus: " -",
      mult: " ✕",
      div: " ÷",
    };

    if (!isNaN(this.second)) {
      return this.second;
    } else {
      return `${this.first}${this.action ? symbols[this.action] : ""}`;
    }
  }

  $_getActionFunc() {
    const actions = {
      plus: (first, second) => first + second,
      minus: (first, second) => first - second,
      mult: (first, second) => first * second,
      div: (first, second) => first / second,
    };
    return actions[this.action];
  }

  $_someIsNaN() {
    return isNaN(this.first) || isNaN(this.second);
  }

  $_everyIsNone() {
    return isNaN(this.first) && isNaN(this.second);
  }

  calculate() {
    if (this.$_everyIsNone()) {
      this.clean_up();
    } else if (this.$_someIsNaN()) {
      this.clean_up(isNaN(this.first) ? this.second : this.first);
    } else if (!this.$_someIsNaN() && Boolean(this.action)) {
      const result = this.$_getActionFunc()(this.first, this.second);
      this.clean_up(result);
    } else {
      this.clean_up();
    }
  }

  setAction(action) {
    if (Boolean(this.action) && !this.$_someIsNaN()) {
      this.calculate();
      this.action = action;
    } else {
      this.action = action;
    }
  }

  numberPressed(number) {
    if (!Boolean(this.action)) {
      if (this.first === 0 || isNaN(this.first)) {
        this.first = number;
      } else {
        this.first = Number("" + this.first + number);
      }
    } else {
      if (isNaN(this.second) || this.second === 0) {
        this.second = number;
      } else {
        this.second = Number("" + this.second + number);
      }
    }
  }
}

export default {
  data: () => ({
    calculator: new Calculator(),
  }),
};
</script>