<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="screen">
      <input v-model.number="operand1" type="text" />
      {{ operator }}
      <input v-model.number="operand2" type="text" />
      =>
      <input v-model.number="result" type="text" readonly />
      <div v-if="error" class="error">{{ error }}</div>
    </div>
    <div class="keyboard">
      <div class="row">
        <button v-for="(item, index) in operators" :key="index" @click="getResult(item)">{{ item }}</button>
      </div>
    </div>
    <input type="checkbox" id="checkbox" v-model="checked" class="checkbox" />
    <label for="checkbox">Отобразить экранную клавиатуру</label>
    <div v-if="checked" class="digits">
      <template class="digits">
        <button v-for="(digit, index) of digits" :key="index" @click="getDigit(digit)">
          {{ digit }}
        </button>
        <button @click="clear">DEL</button>
      </template>
    </div>
    <br />
    <div class="radio">
      <input type="radio" id="one" value="one" v-model="picked" />
      <label for="one">Операнд 1</label>
      <input type="radio" id="two" value="two" v-model="picked" />
      <label for="two">Операнд 2</label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      operand1: 0,
      operand2: 0,
      operator: '+',
      result: 0,
      error: '',
      operators: ['+', '-', '*', '/', '**', '//'],
      digits: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
      checked: "",
      picked: "",
    }
  },
  methods: {
    getResult(operator) {
      this.error = '';
      const operand_1 = Number(this.operand1);
      const operand_2 = Number(this.operand2);
      switch (operator) {
        case '+':
          this.result = operand_1 + operand_2;
          break;
        case '-':
          this.result = operand_1 - operand_2;
          break;
        case '*':
          this.result = operand_1 * operand_2;
          break;
        case '/':
          if (operand_2 == 0) {
            this.error = 'На ноль делить нельзя'
            return 'error'
          } else {
            this.result = operand_1 / operand_2;
            break;
          }
        case '**':
          this.result = operand_1 ** operand_2;
          break;
        case '//':
          this.result = Math.floor(operand_1 / operand_2);
          break;
        default:
          this.result = "Error";
          break;
      }
    },
    getDigit(digit) {
      if (this.picked === "one") {
        +(this.operand1 += digit);
      } else if (this.picked === "two") {
        +(this.operand2 += digit);
      }
    },
    clear() {
      // console.log(this.operand1.split("").pop());
      if (this.operand1 !== 0 && this.picked === "one") {
        +(this.operand1 = this.operand1.slice(0, -1));
      } else if (this.operand2 !== 0 && this.picked === "two") {
        +(this.operand2 = this.operand2.slice(0, -1));
      }
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.error {
  margin: 10px;
  padding: 20px;
  border: 1px solid rgb(134, 9, 9);
  background-color: rgb(231, 158, 158);
  color: rgb(134, 9, 9);
}

.keyboard-btn {
  margin: 15px auto;
}

.digits {
  margin: 15px auto;
  max-width: 1300px;
}

.checkbox {
  margin: 0 auto;
  margin-top: 45px;
}

.radio {
  display: block;
  margin-top: 5px;
}
</style>

