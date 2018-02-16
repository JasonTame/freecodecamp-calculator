<template>
  <div id="app">
    <div class="calculator">
      <div class="calc-area">
        <div class="calc-screen">
          <p>{{calculation.join(" ")}}</p>
          <h3>{{answer}}</h3>
          <h3>{{currNumber}}</h3>
        </div>
      </div>
      <div class="calc-buttons">
        <button v-bind:class="[{longButton: option.isLong}, {operatorColor: option.type == 'operator'}, {cancel : option.display == 'C'} ]" v-for="option in calcOptions" class="calc-button" @click="updateScreen(option)">{{option.display}}</button>
      </div>
    </div>
    <div class = "push"></div>
    <h1 class = "footer">Javascript Calculator App by Jason Tame</h1>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      calculation: [],
      answer: null,
      calcOptions: [
        { type: "command", display: "C", value: null, isLong: true },
        //{ type: "command", display: "+/-", value: null },
        { type: "operator", display: "/", value: null, isLong: false },
        { type: "number", display: 1, value: 1, isLong: false },
        { type: "number", display: 2, value: 2, isLong: false },
        { type: "number", display: 3, value: 3, isLong: false },
        { type: "operator", display: "X", value: null, isLong: false },
        { type: "number", display: 4, value: 4, isLong: false },
        { type: "number", display: 5, value: 5, isLong: false },
        { type: "number", display: 6, value: 6, isLong: false },
        { type: "operator", display: "-", value: null, isLong: false },
        { type: "number", display: 7, value: 7, isLong: false },
        { type: "number", display: 8, value: 8, isLong: false },
        { type: "number", display: 9, value: 9, isLong: false },
        { type: "operator", display: "+", value: null, isLong: false },
        { type: "number", display: 0, value: 0, isLong: true },
        //{ type: "command", display: ".", value: null },
        { type: "operator", display: "=", value: null, isLong: false }
      ],
      currNumber: 0
    }
  },
  methods: {
    updateScreen(option) {
      if (option.type == "number") {
        this.changeValue(option);
      } else if (option.type == "operator") {
        this.useOperator(option.display);
      } else {
        this.runCommand(option.display);
      }
    },
    changeValue(option) {
      if (this.currNumber > 0) {
        this.currNumber = this.currNumber * 10 + option.value;
      } else {
        this.currNumber = option.value;
      }
    },
    useOperator(operator) {
      switch (operator) {
        case "/":
        case "+":
        case "-":
          this.calculation.push(this.currNumber);
          this.calculation.push(operator);
          break;
        case "X":
          this.calculation.push(this.currNumber);
          this.calculation.push("*");
          break;
        case "=":
          //If the number is not odd, then its not a full calculation
          if (this.calculation % 2 != 0) {
            this.calculation.push(this.currNumber);
            this.answer =
              this.calculation.join(" ") +
              " = " +
              eval(this.calculation.join(" "));
            this.calculation = [];
            break;
          }
      }
      this.currNumber = 0;
    },
    runCommand(command) {
      switch (command) {
        case "C":
          this.answer = null;
          this.calcuation = [];
          this.currNumber = 0;
          break;
        case "+/-":
          this.currNumber = this.currNumber * -1;
          break;
      }
    }
  }
}
/*

*/
</script>


<style>
@import url("https://fonts.googleapis.com/css?family=Roboto");

* {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  margin: 0;
}

body {
  background: #BDBDBD;
  padding-bottom: 10px;
  padding-top: 10px;
}

.calculator {
  min-height: 100%;
  max-width: 400px;
  margin: 0 auto;
  border: 2px solid #111;
  border-radius: 5px;
}

.calc-area {
  border-bottom: 2px solid #111;
  border-radius: 0;
  width: 100%;
}

.calc-screen {
  background-color: #fff;
  border-radius: 5px;
  color: #212121;
  font-family: "Roboto";
  font-size: 20px;
  margin: 0 auto;
  padding: 5px;
  text-align: right;
  width: 100%;
}

.calc-buttons {
  background-color: #212121;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 15px;
}

.calc-button {
  background: #212121;
  border: none;
  padding: 15px;
  color: #eee;
  border-radius: 5px;
  font-family: "Roboto";
  font-size: 16px;
  cursor: pointer;
}

.calc-button:hover {
  background-color: #757575
}

.longButton {
  border: 1px solid #fff;
  grid-column: span 3;
}

.cancel:hover {
  background-color: #FF5252;
  border: 1px solid #FF5252;
}

.operatorColor {
  background-color: #388E3C;
}

.operatorColor:hover {
  background-color: #C8E6C9;
  color: #212121;
}

.footer,
.push {
  height: 50px;
}

.footer {
  color: #212121;
  font-family: 'Roboto';
  text-align: center;
}
</style>
