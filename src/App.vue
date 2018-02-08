<template>
<div id="app">
  <div class = "container">
    <h1>Calculator App</h1>
    <div class = "calcScreen">
      <h3>{{currNumber}}</h3>
      <p>{{answer}}</p>
    </div>
    <div class = "calcButtons">
      <ul v-for= "option in calcOptions">
        <li><button @click = "updateScreen(option)">{{option.display}}</button></li>
      </ul>
    </div>
  </div>
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
        {type: "command", display: 'C', value: null},
        {type: 'command', display: '+/-', value: null },
        {type: 'operator', display: "/", value: null },
        {type: 'number', display: 1, value: 1 },
        {type: 'number', display: 2, value: 2 },
        {type: 'number', display: 3, value: 3 },
        {type: 'operator', display: "X", value: null },
        {type: 'number', display: 4, value: 4 },
        {type: 'number', display: 5, value: 5 },
        {type: 'number', display: 6, value: 6 },
        {type: 'operator', display: "-", value: null },
        {type: 'number', display: 7, value: 7 },
        {type: 'number', display: 8, value: 8 },
        {type: 'number', display: 9, value: 9 },
        {type: 'number', display: "+", value: null},
        {type: 'number', display: 0, value: 0 },
        {type: 'command', display: ".", value: null },
        {type: 'operator', display: "=", value: null },
      ],
      currNumber: 0
    }
  },
  methods: {
    updateScreen(option) {
      if(option.type == "number") {
        this.changeValue(option);
      }
      else if(option.type == "operator") {
        this.useOperator(option.display);
      }
      else {
        this.runCommand(option.display);
      }
    },
    changeValue(option) {
        if(this.currNumber > 0) {
          this.currNumber = this.currNumber * 10 + option.value;
        }
        else {
          this.currNumber = option.value;
        }
    },
    useOperator(operator) {
      switch(operator) {
        case "/":
        case "+":
        case "-":
          this.calculation.push(this.currNumber);
          this.calculation.push(operator);
          break;
        case "X":
          this.calculation.push(this.currNumber);
          this.calculation.push("*");
        case "=":
          this.calculation.push(this.currNumber);
          this.answer = eval(this.calculation.join(" "));
          this.calculation = [];
          break;
      }
      this.currNumber = 0;
    },
    runCommand(command) {

    }
  }
}
/*

*/
</script>


<style>

/*element styles*/
li {
  list-style: none;
}












/*------------------------------*/
/*Class styles*/










/*------------------------------*/

  .container {
    align-items: center;
    background-color: mistyrose;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    margin: 0 auto;
    max-width: 60%;
  }


  .calcButtons {
    display: flex;
    flex-wrap: wrap;
    justify-content: center
  }



  .calcButtons button {
    background-color: #22b8cf;
    height: 150px;
    margin: 20px;
    width: 150px;
  }

  .calcButtons button:hover {
    opacity: 0.8;
  }


</style>
