

<template>
  <div>
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="display">
      <input v-model.number="operand1" />
      <input v-model.number="operand2" />
      = {{ result }}
    </div>
    <div class="keyboard">
      <button
        v-for="operand in operands"
        v-bind:key="operand"
        v-bind:title="operand"
        v-bind:disabled="operand1 === '' || operand2 === ''"
        @click="calculate(operand)"
      >
        {{ operand }}
      </button>

      <div class="keyboard">
        <input type="checkbox" id="keyboard" v-model="keyboardShow" />
        <label for="keyboard">Отобразить экранную клавиатуру</label>
        <div class="keys" v-show="keyboardShow">
          <button v-for="key in keys" :key="key" @click="activeKey(key)">
            {{ key }}
          </button>
          <br />
          <input
            type="radio"
            id="op1"
            name="operands"
            value="operand1"
            v-model="operandNum"
            :checked="operandNum === 'operand1'"
          />
          <label for="op1">Операнд 1</label>
          <input
            type="radio"
            id="op2"
            name="operands"
            value="operand2"
            v-model="operandNum"
            :checked="operandNum === 'operand1'"
          />
          <label for="op2">Операнд 2</label>
        </div>
      </div>
    </div>

    <div v-show="error">Ошибка! {{ error }}</div>
    <!-- <div class="logs">
   <div v-for="(log, id) in logs" v-bind:key="id">{{ log }}</div>
    </div> -->
  </div>
</template>
 
<script>
export default {
  name: "Calculator",
  data() {
    return {
      operand1: 0,
      operand2: 0,
      result: 0,
      error: "",
      operands: ["+", "-", "*", "/", "^", "%%"],
      logs: {}, // здесь будем хранить наши логи
      keys: ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "←", "C"],
      keyboardShow: false,
    };
  },

  methods: {
    add() {
      this.result = +this.operand1 + +this.operand2;
    },
    substract() {
      this.result = this.operand1 - this.operand2;
    },
    divide() {
      const { operand1, operand2 } = this;
      if (operand2 === 0) {
        this.error = "Делить на 0 нельзя!";
      } else {
        this.result = operand1 / operand2;
      }
    },

    multiply() {
      this.result = this.operand1 * this.operand2;
    },
    exponentiation() {
      this.result = Math.pow(this.operand1, this.operand2);
    },
    integerDivision() {
      this.result = Math.floor(this.operand1 / this.operand2);
    },
    calculate(operation = "+") {
      this.logs[
        Date.now()
      ] = `${this.operand1}${operation}${this.operand2}=${this.result}`;
      this.error = "";
      switch (operation) {
        case "+":
          this.add();
          break;
        case "-":
          this.substract();
          break;
        case "*":
          this.multiply();
          break;
        case "/":
          this.divide();
          break;
        case "^":
          this.exponentiation();
          break;
        case "%%":
          this.integerDivision();
          break;
      }
    },
    activeKey(key) {
      const num = +key;
      if (isFinite(num)) {
        this.numberActive(num);
        return;
      }
      switch (key) {
        case "←":
          this.backspace();
          break;
        case "C":
          this.clear();
          break;
      }
    },
    numberActive(num) {
      const operand = this.operandNum;
      if (this[operand] !== 0) {
        this[operand] = +(this[operand].toString() + num);
      } else {
        this[operand] = num;
      }
    },
    backspace() {
      const operand = this.operandNum;
      if (this[operand] !== 0) {
        this[operand] = +this[operand].toString().slice(0, -1);
      }
    },
    clear() {
      this.operand1 = 0;
      this.operand2 = 0;
      this.result = 0;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 100px;
}
</style>
