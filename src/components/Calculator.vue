<template>
  <div class="calculator">
    <div class="display">{{previous}}</div>
    <div class="display">{{output}}</div>
    <div class="btn operator" @click="initialize">AC</div>
    <div class="btn operator" @click="sign">+/-</div>
    <div class="btn operator" @click="percent">%</div>
    <div class="btn operator" @click="divide">/</div>
    <div class="btn" @click="append(7)">7</div>
    <div class="btn" @click="append(8)">8</div>
    <div class="btn" @click="append(9)">9</div>
    <div class="btn operator" @click="multiply">X</div>
    <div class="btn" @click="append(4)">4</div>
    <div class="btn" @click="append(5)">5</div>
    <div class="btn" @click="append(6)">6</div>
    <div class="btn operator" @click="minus">-</div>
    <div class="btn" @click="append(1)">1</div>
    <div class="btn" @click="append(2)">2</div>
    <div class="btn" @click="append(3)">3</div>
    <div class="btn operator" @click="add">+</div>
    <div class="btn zero" @click="append(0)">0</div>
    <div class="btn operator" @click="dot">.</div>
    <div class="btn operator" @click="equal">=</div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      previous: null,
      output: `0`,
      operator: null,
      operatorClicked: false
    };
  },
  methods: {
    // initialize method returns value of output to zero
    initialize() {
      this.output = `0`;
      this.previous = null;
      this.operator = null;
      this.operatorClicked = false;
    },
    // percent method divides output value by 100
    percent() {
      this.output = `${parseFloat(this.output) / 100}`;
    },
    // sign added to a number if positive or negative
    sign() {
      this.output =
        this.output.charAt(0) === "-"
          ? this.output.slice(1)
          : `-${this.output}`;
    },
    // To apppend numbers as they are typed
    append(value) {
      if (this.operatorClicked === true) {
        this.operatorClicked = false;
      }

      // if value is zero and the current output is also zero, then output returns as normal to prevent duplicate values.
      if (value === 0 && (this.output === "0" || this.output === "-0")) {
        this.output = "0";
      } else if (
        value === "." &&
        (this.output === "0" || this.output === "-0")
      ) {
        // if value is a dot and output = 0 or -0, the dot is appended.
        this.output = `${this.output}${value}`;
      } else if (value && (this.output === "0" || this.output === "-0")) {
        // if value is true and output is zero or -0, then output becomes the value.
        this.output = `${value}`;
      } else {
        // else value is appended.
        this.output = `${this.output}${value}`;
      }
    },
    // a dot is appended if not found in the current output
    dot() {
      if (this.output.indexOf(".") === -1) {
        this.append(".");
      }
    },
    // To set the Previous Value
    setPrevious() {
      // if previous is defined  and output is also, then equal function is called by hoisting
      if (this.previous && this.output) {
        this.equal();
      }
      this.operatorClicked = true;
      this.previous = this.output;
      this.output = "0";
    },
    // Add
    add() {
      this.setPrevious();
      this.operator = (a, b) => a + b;
    },
    // Minus
    minus() {
      this.setPrevious();
      this.operator = (a, b) => a - b;
    },
    // Divide
    divide() {
      this.setPrevious();
      this.operator = (a, b) => a / b;
    },
    // Multiply
    multiply() {
      this.setPrevious();
      this.operator = (a, b) => a * b;
    },
    //  Equal
    equal() {
      // The operator is given values
      let equal = this.operator(
        parseFloat(this.previous),
        parseFloat(this.output)
      );
      if (isNaN(equal) || equal === Infinity || equal === -Infinity) {
        this.initialize();
        return alert("Math is not computable");
      }
      this.output = `${equal}`;
      this.previous = null; // previous is set back to null
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  width: 20rem;
  margin: 10rem auto;
}

.calculator div {
  border: solid 1px grey;
  text-align: center;
  padding-top: 1rem;
  font-weight: bolder;
}
.display {
  grid-column: 1/5;
  background-color: rgb(13, 87, 156);
  color: white;
  font-size: 2rem;
  padding-top: 0.2rem !important;
}

.zero {
  grid-column: 1/3;
  text-align: center;
}

.btn:active {
  background-color: black;
  color: white;
}

.operator {
  color: rgb(13, 87, 156);
}
</style>
