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
  <div class="push"></div>
  <h1 class="footer">Javascript Calculator App by Jason Tame</h1>
</div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      calculation: [],
      answer: null,
      calcOptions: [{
          type: "command",
          display: "C",
          isLong: true
        },
        {
          type: "command",
          display: "+/-"
        },
        {
          type: "operator",
          display: "/"
        },
        {
          type: "number",
          display: "1"
        },
        {
          type: "number",
          display: "2"
        },
        {
          type: "number",
          display: "3"
        },
        {
          type: "operator",
          display: "X"
        },
        {
          type: "number",
          display: "4"
        },
        {
          type: "number",
          display: "5"
        },
        {
          type: "number",
          display: "6"
        },
        {
          type: "operator",
          display: "-"
        },
        {
          type: "number",
          display: "7"
        },
        {
          type: "number",
          display: "8"
        },
        {
          type: "number",
          display: "9"
        },
        {
          type: "operator",
          display: "+"
        },
        {
          type: "number",
          display: "0",
          isLong: true
        },
        {
          type: "command",
          display: "."
        },
        {
          type: "operator",
          display: "="
        }
      ],
      currNumber: "0",
      decimal: false,
      //Boolean which determines whether a number has a decimal place already or not.
      isFloat: false,
      //Boolean which stops an operator from being pressed twice
      operatorSelected: false
    }
  },
  methods: {
    updateScreen(option) {
      if (option.type == "number") {
        this.changeValue(option.display);
      } else if (option.type == "operator") {
        this.useOperator(option.display);
      } else {
        this.runCommand(option.display);
      }
    },
    changeValue(number) {
      this.operatorSelected = false;
      /*Check if this is the start of a new calculation. If it is, remove
        the previous answer from the screen*/
      if (this.answer) {
        this.answer = null;
      }

      if (!this.decimal) {
        if (this.currNumber !== "0") {
          this.currNumber += number;
        } else {
          this.currNumber = number;
        }
      } else {
        this.currNumber = this.currNumber + number;
      }
    },
    useOperator(operator) {
      if(!this.operatorSelected) {
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
              this.answer = this.calculation.join(" ") + " = " + eval(this.calculation.join(" ")).toFixed(2);
              this.calculation = [];
              break;
            }
        }
      }

      this.currNumber = "0";
      this.isFloat = false;
      this.operatorSelected = true;
    },
    runCommand(command) {
      switch (command) {
        case "C":
          this.answer = null;
          this.calculation = [];
          this.currNumber = "0";
          break;
        case "+/-":
          if(eval(this.currNumber) > 0) {
            this.currNumber = "-" + this.currNumber;
          }
          else {
            this.currNumber = this.currNumber.substr(1);
          }
          break;
        case ".":
          if (!this.isFloat) {
            this.currNumber = this.currNumber + ".";
            this.decimal = true;
            this.isFloat = true;
          }
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
  grid-column: span 2;
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
