<script setup>
import { defineEmits } from 'vue'

// const props = defineProps({
//   message: String,
// })
// const clickEvent = () => {
//   console.log('자식컴포넌트의 버튼 클릭됨')
// }
// const emit = defineEmits(['messageSent'])
// const sendMessage = () => {
//   emit('messageSent', 'Hello from Child!')
// }
</script>
<!-- <p>{{ message }}</p>
<button @click="sendMessage">send message</button> -->
<template>
  <div>
    <header>
      <slot name="header"></slot>
    </header>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
