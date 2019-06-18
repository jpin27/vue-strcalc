<template>
  <div id="app">
    <img src="./assets/logo.png" style="width:150px;height:150px;border:0;">
    <h2>Result of string computation is: {{ computeResult }}</h2>
    <textarea rows="4" cols="30" id="input" v-model="userInput"></textarea>
    <br>
    <button id='exec-button' @click="add">Execute</button>
    <br>
    <p>{{ exceptionTicker }}</p>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      userInput: "",
      exceptionTicker: "",
      computeResult: 0,
      intList: [],
      delimiterList: []
    }
  },
  methods: {
    add() {
      // reset the variables everytime the button is clicked
      this.intList = [];
      this.delimiterList = [];
      this.computeResult = 0;
      this.exceptionTicker = "";

      // input string first split: the control string ends at the first instance
      // of a newline, then tokenize the control string to get the delimiters
      var firstSlice = this.userInput.split("\n", 1);
      var tokenizedDelimiters = firstSlice[0].split(",");
      for (var i in tokenizedDelimiters) {
        this.delimiterList.push(tokenizedDelimiters[i]);
      }

      // delimiterList[0] will still have "//" in it. take it off, and
      // everything to the left of the double slashes.
      var firstDelimiter = this.delimiterList[0];
      this.delimiterList[0] = firstDelimiter.substring(
        firstDelimiter.indexOf("//") + 2
      );

      // the user input string will be cleaved in twain!
      var slicePoint = firstSlice[0].length;
      var secondSlice = this.userInput.slice(slicePoint);

      // clean up secondSlice - remove the leading newline and anything 
      // after the last number.
      secondSlice = secondSlice.substring(
        firstDelimiter.indexOf("\n") + 2
      );

      // explode secondSlice - put operators in one array and operands in another
      var operatorArray = secondSlice.match(/\D+/g)
      var operandArray = secondSlice.match(/(-|)\d+/g)

      // edge case: check if first operand is < 1000 and set to 0 if it is not.
      // also, we always add the first element to the result.
      if(operandArray[0] > 1000) operandArray[0] = 0;
      if(operandArray[0] >= 0) this.computeResult += parseInt(operandArray[0]);

      // declare flags to check for negative input
      // TODO: catch multiple negative inputs
      var hasNegative = false; var negNumber = 0;
      for(i in operandArray) {
        if(parseInt(operandArray[i]) < 0) {
          hasNegative = true; 
          negNumber = operandArray[i];
        }
      }

      // abort the operation when a negative input is encountered.
      if(hasNegative){
        this.exceptionTicker = "Negatives not allowed: on input " + negNumber;
        this.computeResult = 0;
      } else {
        // compare each operator with the delimiter list. if it is a legitimate
        // operator, add operands on both sides of the operator to the total.
        for(i = 0; i < operatorArray.length; i++){
          // operatorArray will always have one less element than operandArray. 
          // If it is not the case, there are extra symbols to the right of the 
          // last operand. We ignore those.
          if(i != operandArray.length){
            var x = this.delimiterList.indexOf(operatorArray[i]);
            if(x != -1){
              if(operandArray[i+1] > 1000) operandArray[i+1] = 0;
              this.computeResult += parseInt(operandArray[i+1]);
            }
          }
        }
      }
      

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
