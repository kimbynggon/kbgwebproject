<script setup>
import { ref } from 'vue'

const clickCount = ref(0)
const mouseX = ref(0)
const mouseY = ref(0)
const boxColor = ref('#ffffff')
const keyMessage = ref('')
const textColor = ref('#ffffff')

const increment = () => {
  clickCount.value++
}
const resetclick = () => {
  clickCount.value = 0
}
const updateMousePosition = (event) => {
  mouseX.value = event.offsetX
  mouseY.value = event.offsetY
}
const changeBackgroundRandom = () => {
  const randombgColor = '#' + Math.floor(Math.random() * 16777215).toString(16)
  const randomtextColor = '#' + Math.floor(Math.random() * 16777215).toString(16)
  boxColor.value = randombgColor
  textColor.value = randomtextColor
  // console.log('배경화면 클릭됨')
}
const handleKeyDown = (event) => {
  if (event.key === 'Enter') {
    keyMessage.value = '엔터키를 눌렀습니다!'
    // console.log('엔터키를 눌렀습니다!')
  } else if (event.key === 'Escape') {
    keyMessage.value = 'ESC키를 눌렀습니다!'
    // console.log('ESC키를 눌렀습니다!')
  } else if (event.key === ' ') {
    keyMessage.value = '스페이스바를 눌렀습니다!'
    // console.log('스페이스바를 눌렀습니다!')
  }
  // } else if (event.key.length === 1) {
  //   keyMessage.value = `"${event.key}" 키를 눌렀습니다!`
  //   // console.log(`"${event.key}" 키를 눌렀습니다!`)
  // } else {
  //   keyMessage.value = `${event.key} 키를 눌렀습니다!`
  //   // console.log(`${event.key} 키를 눌렀습니다!`)
  // }
}
</script>

<template>
  <h1>클릭 횟수 {{ clickCount }}</h1>
  <button @click="increment" @dblclick="resetclick">클릭 버튼</button>
  <div
    @mousemove="updateMousePosition"
    style="width: 400px; height: 400px; border: 1px solid #ffffff; padding: 20px"
  >
    <p>마우스 위치: ({{ mouseX }}, {{ mouseY }})</p>
  </div>
  <div
    @click="changeBackgroundRandom"
    style="
      width: 500px;
      height: 200px;
      border: 1px solid #ffffff;
      padding: 20px;
      margin-top: 20px;
      background-color: boxColor;
    "
    :style="{ backgroundColor: boxColor, color: textColor }"
  >
    마우스 클릭시 배경화면이 바뀝니다.
  </div>
  <div
    @keydown="handleKeyDown"
    @keyup="handleKeyUp"
    tabindex="0"
    style="padding: 20px; margin-top: 20px; width: 200px; height: 200px"
  >
    이 영역을 클릭한 후 키보드를 눌러보세요
  </div>

  <div>{{ keyMessage }}</div>

  <router-view></router-view>
</template>
