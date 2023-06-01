<template>
    <div class="calculator">
      <h1>{{ msg }}</h1>
      <table>
        <tbody>
          <tr>
            <td colspan = 4>{{ currentNumber }}</td>
          </tr>
          <tr>
            <td @click="handleClick('C')">C</td>
            <td @click="handleClick('+/-')">+/-</td>
            <td @click="handleClick('%')">%</td>
            <td @click="handleClick('/')">/</td>
          </tr>
          <tr>
            <td @click="handleClick('7')">7</td>
            <td @click="handleClick('8')">8</td>
            <td @click="handleClick('9')">9</td>
            <td @click="handleClick('*')">*</td>
          </tr>
          <tr>
            <td @click="handleClick('4')">4</td>
            <td @click="handleClick('5')">5</td>
            <td @click="handleClick('6')">6</td>
            <td @click="handleClick('-')">-</td>
          </tr>
          <tr>
            <td @click="handleClick('1')">1</td>
            <td @click="handleClick('2')">2</td>
            <td @click="handleClick('3')">3</td>
            <td @click="handleClick('+')">+</td>
          </tr>
          <tr>
            <td @click="handleClick('0')" colspan = 2>0</td>
            <td @click="handleClick('.')">.</td>
            <td @click="calculate()">=</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CalculadoraComponent',
    data() {
      return {
        msg: 'Calculadora',
        currentNumber: '0',
        previousNumber: null,
        operator: null,
        waitingForNextNumber: false
      };
    },
    methods: {
      handleClick(value) {
        if (value === 'C') {
          this.currentNumber = '0';
          this.previousNumber = null;
          this.operator = null;
          this.waitingForNextNumber = false; 
        } else if (value === '%') {
          this.currentNumber = (parseFloat(this.currentNumber) / 100).toStrin();
        } else if (['+', '-', '*', '/'].includes(value)) {
          if (this.operator && this.previousNumber) {
            this.calculate();
          }
          this.operator = value;
          this.previousNumber = this.currentNumber;
          this.waitingForNextNumber = true;
        } else if (value === '=') {
          this.calculate();
          this.waitingForNextNumber = true;
        } else {
          if (this.waitingForNextNumber) {
            this.currentNumber = value;
            this.waitingForNextNumber = false;
          } else {
            if (this.currentNumber === '0' && value !== '.') {
              this.currentNumber = value;
            } else {
              this.currentNumber += value;
            }
          }
        }
      },
      calculate() {
        const num1 = parseFloat(this.previousNumber);
        const num2 = parseFloat(this.currentNumber);
        let result = 0;
  
        switch (this.operator) {
          case '+':
            result = num1 + num2;
            break;
          case '-':
            result = num1 - num2;
            break;
          case '*':
            result = num1 * num2;
            break;
          case '/':
            result = num1 / num2;
            break;
        }
  
        this.currentNumber = result.toString();
        this.previousNumber = null;
        this.operator = null;
      }
    }
  };
  </script>
  
  <style scoped>
  .calculator {
  margin-top: 60px;
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

table {
  margin: 0 auto;
}

td {
  width: 50px;
  height: 50px;
  border: 1px solid #ccc;
  cursor: pointer;
  border-radius: 10%;
  animation: rgbLights 2.6s infinite;
  user-select: none;
}
td:hover {
  background-color: #CFD8DC;
}

td:active {
  background-color: #5C6BC0;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

@keyframes rgbLights {
  0%, 100% {
    border-color: #FF6699;
  }
  10%, 90% {
    border-color: #FF3366;
  }
  20%, 80% {
    border-color: #FF33CC;
  }
  30%, 70% {
    border-color: #CC33FF;
  }
  40%, 60% {
    border-color: #9933FF;
  }
  50% {
    border-color: #6633FF;
  }
}
  </style>
  