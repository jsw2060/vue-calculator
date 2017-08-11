<template>
  <div class="calculator">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/meyer-reset/2.0/reset.min.css">
    <div class="calculator-display" v-model="displayValue">{{ displayValue }}</div>
    <div class="calculator-keypad">
      <div class="input-keys">
        <div class="digit-keys">
          <button class="calculator-key key-clear" @click="clearDisplay()">AC</button>
          <button class="calculator-key key-sign" @click="toggleSign()">±</button>
          <button class="calculator-key key-percent" @click="inputPercent()">%</button>
          <button class="calculator-key key-9" @click="inputDigit(9)">9</button>
          <button class="calculator-key key-8" @click="inputDigit(8)">8</button>
          <button class="calculator-key key-7" @click="inputDigit(7)">7</button>
          <button class="calculator-key key-6" @click="inputDigit(6)">6</button>
          <button class="calculator-key key-5" @click="inputDigit(5)">5</button>
          <button class="calculator-key key-4" @click="inputDigit(4)">4</button>
          <button class="calculator-key key-3" @click="inputDigit(3)">3</button>
          <button class="calculator-key key-2" @click="inputDigit(2)">2</button>
          <button class="calculator-key key-1" @click="inputDigit(1)">1</button>
          <button class="calculator-key key-0" @click="inputDigit(0)">0</button>
          <button class="calculator-key key-dot" @click="inputDot()">ㆍ</button>
        </div>
        <div class="operator-keys">
          <button class="calculator-key key-divide" @click="performOperation('/')">÷</button>
          <button class="calculator-key key-multiply" @click="performOperation('*')">×</button>
          <button class="calculator-key key-subtract" @click="performOperation('-')">－</button>
          <button class="calculator-key key-add" @click="performOperation('+')">＋</button>
          <button class="calculator-key key-equals" @click="performOperation('=')">＝</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      value: null,
      displayValue: '0',
      waitingForOperand: false,
      operator: null
    }
  },
  methods: {
    inputDigit (digit) {
      if (this.waitingForOperand) {
        this.displayValue = String(digit)
        this.waitingForOperand = false
      } else {
        this.displayValue = this.displayValue === '0' ? String(digit) : this.displayValue + digit
      }
    },
    inputDot () {
      if (this.waitingForOperand) {
        this.displayValue = '.'
        this.waitingForOperand = false
      } else if (this.displayValue.indexOf('.') === -1) {
        this.displayValue = this.displayValue + '.'
        this.waitingForOperand = false
      }
    },
    clearDisplay () {
      this.displayValue = '0'
    },
    toggleSign () {
      this.displayValue = this.displayValue.charAt(0) === '-' ? this.displayValue.substr(1) : '-' + this.displayValue
    },
    inputPercent () {
      const value = parseFloat(this.displayValue)

      this.displayValue = String(value / 100)
    },
    performOperation (nextOperator) {
      const nextValue = parseFloat(this.displayValue)
      const operations = {
        '/': (prevValue, nextValue) => prevValue / nextValue,
        '*': (prevValue, nextValue) => prevValue * nextValue,
        '+': (prevValue, nextValue) => prevValue + nextValue,
        '-': (prevValue, nextValue) => prevValue - nextValue,
        '=': (prevValue, nextValue) => nextValue
      }
      if (this.value === null) {
        // No previous value, hit a operator key
        this.value = nextValue
      } else if (this.operator) {
        const currentValue = this.value || 0
        const computedValue = operations[this.operator](currentValue, nextValue)
        this.value = computedValue
        this.displayValue = String(computedValue)
      }
      this.waitingForOperand = true
      this.operator = nextOperator
    }
  }
}
</script>
