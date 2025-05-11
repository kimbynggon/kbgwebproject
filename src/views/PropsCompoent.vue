<script setup>
import { ref } from 'vue'
import ChildCompoent from './ChildCompoent.vue'

// const parentMessage = ref('Hello from Parent!')
// const receivedmessage = ref('')

// const receiveMessage = (message) => {
//   console.log('부모컴포넌트에서 받은 메시지:', message)
//   receivedmessage.value = message
// }
</script>

<!-- <ChildCompoent @messagesent="receiveMessage" />
<p>{{ receivedmessage }}</p> -->
<template>
  <child-compoent>
    <template v-slot:header>
      <h1>header slot content</h1>
    </template>
    <template v-slot:footer>
      <h1>footer slot content</h1>
    </template>
  </child-compoent>
</template>
