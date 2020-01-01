<template>
  <div id="app">
    <div class="container">
      <p class="right">{{ currentNumber || 0 }}</p>
      <div class="group">
        <div class="left-section">
          <calculatorButtons
            :buttons="buttonValues"
            @perform-action="handleClick"
          />
        </div>
        <div class="right-section">
          <calculatorButtons
            :buttons="functionValues"
            @perform-action="handleClick"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import calculatorButtons from "./components/calculatorButtons";

export default {
  name: "app",
  components: {
    calculatorButtons
  },
  data() {
    return {
      currentNumber: "",
      previousNumber: null,
      buttonValues: [
        "C",
        "+/-",
        "%",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "0",
        ".",
        "√"
      ],
      functionValues: ["÷", "X", "-", "+", "="],
      operatorClicked: false,
      count: 5
    };
  },
  methods: {
    handleClick(value) {
      if (value === "C") {
        this.clear();
      } else if (value == "X") {
        this.checkDot();
        this.multiply();
      } else if (value == "-") {
        this.checkDot();
        this.subtract();
      } else if (value == "+") {
        this.checkDot();
        this.add();
      } else if (value == "÷") {
        this.checkDot();
        this.divide();
      } else if (value == "%") {
        this.checkDot();
        this.percentage();
      } else if (value == ".") {
        this.checkDot();
        this.appendDot();
      } else if (value == "=") {
        this.appendDot();
        this.equal();
      } else if (value == "+/-") {
        this.checkDot();
        this.toggleSign();
      } else if (value == "√") {
        this.checkDot();
        this.sqrt();
      } else {
        this.appendValue(value);
      }
    },
    checkDot() {
      if (this.currentNumber[this.currentNumber.length - 1] === ".") {
        let newNumber = this.currentNumber.toString();
        const arr = newNumber.split("");
        arr.pop();
        this.currentNumber = arr.join("");
      }
    },
    clear() {
      this.currentNumber = "";
    },
    toggleSign() {
      this.currentNumber = parseFloat(this.currentNumber) * -1;
    },
    percentage() {
      this.currentNumber = parseFloat(this.currentNumber) / 100;
    },
    appendValue(value) {
      if (this.operatorClicked) {
        this.currentNumber = "";
        this.operatorClicked = false;
      }
      if (this.currentNumber.length <= this.count) {
        this.currentNumber += value;
      }
    },
    appendDot() {
      if (this.currentNumber.indexOf(".") === -1) {
        this.appendValue(".");
      }
    },
    setPrevious() {
      this.previousNumber = this.currentNumber;
      this.operatorClicked = true;
    },
    sqrt() {
      this.currentNumber = Math.sqrt(this.currentNumber).toFixed(8);
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    subtract() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    equal() {
      this.currentNumber = `
      ${this.operator(
        parseFloat(this.previousNumber),
        parseFloat(this.currentNumber)
      )}
      `;
      this.previousNumber = null;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  font-family: "Roboto", sans-serif;
}

body {
  background: #1f2227;
}

.container {
  width: 300px;
  height: 420px;
  margin: 50px auto;
  background: #5a5d7a;
  border-radius: 4px;
  position: relative;
}

.right {
  text-align: right;
  font-size: 40px;
  padding: 15px;
  color: #fff;
}

.left-section {
  width: 75%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  position: absolute;
  bottom: 0;
  left: 0;
}

.left-section button {
  width: 33.33%;
  height: 70px;
  border: 0;
  border-bottom: 0.4px solid #3f4042;
  padding: 20px;
  font-weight: bold;
  font-size: 20px;
  background-color: #fff;
  color: #1f2227;
}

.right-section {
  width: 25%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  position: absolute;
  bottom: 0;
  right: 0;
  margin: 0;
}

.right-section button {
  width: 100%;
  height: 70px;
  border: 0;
  padding: 20px;
  font-weight: bold;
  color: #1f2227;
  border-bottom: 0.4px solid #3f4042;
  background-color: #dfdf18;
}

.group {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  width: 100%;
}
</style>
