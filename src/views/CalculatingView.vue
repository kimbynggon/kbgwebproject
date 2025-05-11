<script setup>
import { ref, computed } from 'vue'
import CustomAlert from '../components/AlertView.vue'

// 변수
const inputNumber = ref('0')
const operator = ref('')
const history = ref('')
const previousInput = ref('')
const resetInputNumer = ref(false)
const customAlertRef = ref(null)
const myHistory = ref([])
const showAreaa = ref(false)

const toggleHistory = () => {
  showAreaa.value = !showAreaa.value
}

// 계산기의 숫자자를 입력해주는곳
const mainNumber = (number) => {
  if (resetInputNumer.value) {
    inputNumber.value = number
    resetInputNumer.value = false
  } else {
    if (inputNumber.value === '0') {
      inputNumber.value = number
    } else {
      inputNumber.value += number
    }
  }
  // 연산자 입력 후 숫자 입력 시 history 업데이트
  // if (operator.value && previousInput.value) {
  //   const operatorIcon =
  //     operator.value === '*' ? '×' : operator.value === '/' ? '÷' : operator.value
  //   history.value = `${previousInput.value} ${operatorIcon} ${inputNumber.value}`
  // }
}
// 계산기의 점을 입력해주는 곳
const inputComma = () => {
  if (resetInputNumer.value) {
    inputNumber.value = '0.'
    resetInputNumer.value = false
  } else if (!inputNumber.value.includes('.')) {
    inputNumber.value += '.'
  }
}
// 계산기의 입력을 표시해주는곳곳
const numberContent = computed(() => {
  if (inputNumber.value.includes('.')) {
    const [integerPart, decimalPart] = inputNumber.value.split('.')
    const formattedInteger = Number(integerPart).toLocaleString()
    return formattedInteger + '.' + decimalPart
  }
  return Number(inputNumber.value).toLocaleString()
})
// 계산기내 입력된값을 한번에 지워주는곳
const clear = () => {
  inputNumber.value = '0'
  operator.value = ''
  previousInput.value = ''
  resetInputNumer.value = false
  history.value = ''
  myHistory.value = [] // 히스토리 초기화
}
// 계산기내 입력된값을 한개씩 지워주는곳
const deleteNumber = () => {
  if (inputNumber.value.length > 1) {
    inputNumber.value = inputNumber.value.slice(0, -1)
  } else {
    inputNumber.value = '0'
  }

  // 숫자 삭제 시 history 업데이트 (연산자만 있는 경우 제외)(미완성)
  if (operator.value && previousInput.value && inputNumber.value !== '0') {
    const operatorIcon =
      operator.value === '*' ? '×' : operator.value === '/' ? '÷' : operator.value
    history.value = `${previousInput.value} ${operatorIcon} ${inputNumber.value}`
  } else if (!previousInput.value) {
    history.value = ''
  }
}

// 사칙연산 입력해주는곳
const insertOperator = (op) => {
  if (operator.value !== '') {
    calculate()
  }
  previousInput.value = inputNumber.value
  operator.value = op
  resetInputNumer.value = true

  // 히스토리에 연산자 기록
  const operatorIcon = op === '*' ? '×' : op === '/' ? '÷' : op
  history.value = `${previousInput.value} ${operatorIcon}`
  myHistory.value.push(history.value)
}
// 실제 계산해주는곳곳
const calculate = () => {
  if (operator.value === '' || resetInputNumer.value) return

  const prev = parseFloat(previousInput.value.replace(/,/g, ''))
  const cur = parseFloat(inputNumber.value.replace(/,/g, ''))
  let result = 0

  switch (operator.value) {
    case '+':
      result = prev + cur
      break
    case '-':
      result = prev - cur
      break
    case '*':
      result = prev * cur
      break
    case '/':
      if (cur !== 0) {
        result = prev / cur
      } else {
        customAlertRef.value.showAlert('0으로 나누는건 불가능합니다. 다시 입력해주세요')
        clear()
        return
      }
      break
  }
  // 히스토리 기록해주는곳 (아직 미완성)
  const operatorIcon = operator.value === '*' ? '×' : operator.value === '/' ? '÷' : operator.value
  const finalHistory = `${previousInput.value} ${operatorIcon} ${inputNumber.value} = ${Number.isInteger(result) ? result.toString() : result.toFixed(8).replace(/\.?0+$/, '')}`
  history.value = finalHistory
  myHistory.value.push(finalHistory)
  inputNumber.value = Number.isInteger(result)
    ? result.toString()
    : result.toFixed(8).replace(/\.?0+$/, '')
  operator.value = ''
  resetInputNumer.value = true
}

// 키보드 입력 처리해주는곳
const handleKeydown = (event) => {
  const key = event.key
  if (!isNaN(key)) {
    mainNumber(key)
  } else if (key === '.') {
    inputComma()
  } else if (key === 'Enter') {
    calculate()
  } else if (key === 'Backspace') {
    deleteNumber()
  } else if (['+', '-', '*', '/'].includes(key)) {
    insertOperator(key)
  } else if (key === 'Escape') {
    clear()
  }
}
window.addEventListener('keydown', handleKeydown)
</script>
<template>
  <div class="calculator">
    <custom-alert ref="customAlertRef" />
    <div class="display">
      <button class="btn btn-soft btn-info" @click="toggleHistory">!</button>
      <div class="history">{{ history }}</div>
      <div class="result">{{ numberContent }}</div>
    </div>
    <div class="buttons">
      <button class="btn btn-function btn-error" @click="clear">C</button>
      <button class="btn btn-function btn-warning" @click="deleteNumber">⌫</button>
      <button class="btn btn-operator" @click="insertOperator('/')">/</button>

      <button class="btn btn-soft" @click="mainNumber('7')">7</button>
      <button class="btn btn-soft" @click="mainNumber('8')">8</button>
      <button class="btn btn-soft" @click="mainNumber('9')">9</button>
      <button class="btn btn-operator" @click="insertOperator('*')">×</button>

      <button class="btn btn-soft" @click="mainNumber('4')">4</button>
      <button class="btn btn-soft" @click="mainNumber('5')">5</button>
      <button class="btn btn-soft" @click="mainNumber('6')">6</button>
      <button class="btn btn-operator" @click="insertOperator('-')">-</button>

      <button class="btn btn-soft" @click="mainNumber('1')">1</button>
      <button class="btn btn-soft" @click="mainNumber('2')">2</button>
      <button class="btn btn-soft" @click="mainNumber('3')">3</button>
      <button class="btn btn-operator" @click="insertOperator('+')">+</button>

      <button class="btn btn-soft btn-zero" @click="mainNumber('0')">0</button>
      <button class="btn btn-soft" @click="inputComma">.</button>
      <button class="btn btn-equal" @click="calculate">=</button>
    </div>
    <div v-if="showAreaa" class="history-area">
      <h3>History</h3>
      <ul v-if="myHistory.length > 0">
        <li v-for="(item, index) in myHistory" :key="index">{{ item.trim() }}</li>
      </ul>
      <div v-else></div>
      <button @click="myHistory = []" v-if="myHistory.length > 0">Clear History</button>
    </div>
  </div>
</template>

<style scoped>
.btn-info {
  border-radius: 50% !important;
  width: 50px;
  height: 50px !important;
  line-height: 30px;
  font-size: 1rem;
  margin-right: 10px;
  aspect-ratio: 1 / 1;
  transform: translateX(-170px);
}
.calculator {
  width: 450px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}
.display {
  padding: 20px;
  background-color: #1e293b;
  color: white;
  min-height: 60px; /* 높이 조정 */
  display: flex;
  align-items: center;
  justify-content: flex-end;
  flex-direction: column; /* 세로 방향으로 요소 배치 */
}
.history {
  font-size: 1rem;
  color: #9ca3af;
  text-align: right; /* 오른쪽 정렬 */
  width: 100%; /* 전체 너비 차지 */
  overflow: hidden;
  margin-bottom: 5px;
}
.result {
  font-size: 2rem;
  text-align: right;
  width: 100%;
  overflow: hidden;
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
.btn-warning {
  grid-column: span 2;
}
.btn-warning:hover {
  color: #ffffff;
}
.btn-delete:hover {
  color: #ffffff;
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
.btn-equal {
  background-color: #2563eb;
  color: white;
}
.btn-equal:hover {
  background-color: #1d4ed8;
}

.history-area {
  padding: 20px;
}

.history-area h3 {
  margin-top: 0;
  margin-bottom: 10px;
}

.history-area ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.history-area li {
  padding: 5px 0;
  border-bottom: 1px dashed #eee;
  text-align: right; /* 히스토리 아이템 오른쪽 정렬 */
}

.history-area li:last-child {
  border-bottom: none;
}

.history-area button {
  margin-top: 10px;
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  background-color: #dc2626;
  color: white;
  cursor: pointer;
  font-size: 1rem;
  display: block; /* 버튼을 블록 요소로 만들어 가운데 정렬 용이하게 */
  margin-left: auto;
  margin-right: auto;
}

.history-area button:hover {
  background-color: #b91c1c;
}
</style>
