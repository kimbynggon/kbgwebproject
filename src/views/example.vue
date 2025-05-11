<!-- <template>
  <div class="calculator">
    <div class="display">
      <div class="history">{{ history }}</div>
      <div>{{ formattedDisplay }}</div>
    </div>
    <div class="buttons">
      <button class="btn btn-function btn-clear" @click="clear">C</button>
      <button class="btn btn-function" @click="backspace">⌫</button>
      <button class="btn btn-operator" @click="setOperator('/')">/</button>

      <button class="btn btn-number" @click="appendNumber('7')">7</button>
      <button class="btn btn-number" @click="appendNumber('8')">8</button>
      <button class="btn btn-number" @click="appendNumber('9')">9</button>
      <button class="btn btn-operator" @click="setOperator('*')">×</button>

      <button class="btn btn-number" @click="appendNumber('4')">4</button>
      <button class="btn btn-number" @click="appendNumber('5')">5</button>
      <button class="btn btn-number" @click="appendNumber('6')">6</button>
      <button class="btn btn-operator" @click="setOperator('-')">-</button>

      <button class="btn btn-number" @click="appendNumber('1')">1</button>
      <button class="btn btn-number" @click="appendNumber('2')">2</button>
      <button class="btn btn-number" @click="appendNumber('3')">3</button>
      <button class="btn btn-operator" @click="setOperator('+')">+</button>

      <button class="btn btn-number btn-zero" @click="appendNumber('0')">0</button>
      <button class="btn btn-number" @click="appendDecimal">.</button>
      <button class="btn btn-equal" @click="calculate">=</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const currentInput = ref('0')
const previousInput = ref('')
const operator = ref('')
const resetOnNextInput = ref(false)
const history = ref('')

// 3자리마다 콤마 추가하는 포맷팅 함수
const formattedDisplay = computed(() => {
  if (currentInput.value.includes('.')) {
    const [integerPart, decimalPart] = currentInput.value.split('.')
    const formattedInteger = Number(integerPart).toLocaleString()
    return formattedInteger + '.' + decimalPart
  }
  return Number(currentInput.value).toLocaleString()
})

const appendNumber = (number) => {
  if (resetOnNextInput.value) {
    currentInput.value = number
    resetOnNextInput.value = false
  } else {
    if (currentInput.value === '0') {
      currentInput.value = number
    } else {
      currentInput.value += number
    }
  }
}

const appendDecimal = () => {
  if (resetOnNextInput.value) {
    currentInput.value = '0.'
    resetOnNextInput.value = false
  } else if (!currentInput.value.includes('.')) {
    currentInput.value += '.'
  }
}

const setOperator = (op) => {
  if (operator.value !== '') {
    calculate()
  }
  previousInput.value = currentInput.value
  operator.value = op
  resetOnNextInput.value = true

  const operatorSymbol = op === '*' ? '×' : op === '/' ? '÷' : op
  history.value = `${previousInput.value} ${operatorSymbol}`
}

const calculate = () => {
  if (operator.value === '' || resetOnNextInput.value) return

  const prev = parseFloat(previousInput.value.replace(/,/g, ''))
  const current = parseFloat(currentInput.value.replace(/,/g, ''))
  let result = 0

  switch (operator.value) {
    case '+':
      result = prev + current
      break
    case '-':
      result = prev - current
      break
    case '*':
      result = prev * current
      break
    case '/':
      if (current !== 0) {
        result = prev / current
      } else {
        alert('0으로 나눌 수 없습니다')
        clear()
        return
      }
      break
  }

  const operatorSymbol =
    operator.value === '*' ? '×' : operator.value === '/' ? '÷' : operator.value
  history.value = `${previousInput.value} ${operatorSymbol} ${currentInput.value} =`

  currentInput.value = Number.isInteger(result)
    ? result.toString()
    : result.toFixed(8).replace(/\.?0+$/, '')
  operator.value = ''
  resetOnNextInput.value = true
}

const clear = () => {
  currentInput.value = '0'
  previousInput.value = ''
  operator.value = ''
  resetOnNextInput.value = false
  history.value = ''
}

const backspace = () => {
  if (currentInput.value.length > 1) {
    currentInput.value = currentInput.value.slice(0, -1)
  } else {
    currentInput.value = '0'
  }
}

const handleKeydown = (event) => {
  const { key } = event

  if (/^[0-9]$/.test(key)) {
    appendNumber(key)
  } else if (['+', '-', '*', '/'].includes(key)) {
    setOperator(key)
  } else if (key === 'Enter') {
    calculate()
  } else if (key === '.') {
    appendDecimal()
  } else if (key === 'Backspace') {
    backspace()
  } else if (key === 'Escape') {
    clear()
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.calculator {
  width: 100%;
  max-width: 400px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}
.display {
  padding: 20px;
  background-color: #1e293b;
  color: white;
  text-align: right;
  font-size: 2rem;
  min-height: 80px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.history {
  font-size: 1rem;
  color: #9ca3af;
  height: 24px;
  overflow: hidden;
  margin-bottom: 5px;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px;
  background-color: #e5e7eb;
}
.btn {
  border-radius: 0;
  height: 70px;
  font-size: 1.5rem;
  font-weight: bold;
}
.btn-clear {
  grid-column: span 2;
}
.btn-zero {
  grid-column: span 2;
}
.btn-operator {
  background-color: #f59e0b;
  color: white;
}
.btn-operator:hover {
  background-color: #d97706;
}
.btn-number {
  background-color: #f8fafc;
}
.btn-number:hover {
  background-color: #e2e8f0;
}
.btn-function {
  background-color: #d1d5db;
}
.btn-function:hover {
  background-color: #9ca3af;
}
.btn-equal {
  background-color: #2563eb;
  color: white;
}
.btn-equal:hover {
  background-color: #1d4ed8;
}
</style>

