<template>
  <div class="modal" :class="{ 'modal-open': show }">
    <div class="modal-box bg-neutral-800 text-white rounded-xl p-6">
      <div class="flex justify-center mb-4">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-12 w-12 stroke-warning"
          fill="none"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
          />
        </svg>
      </div>
      <h3 class="font-bold text-lg text-center mb-2">Warning!</h3>
      <p class="text-center mb-4">{{ message }}</p>
      <div class="modal-action justify-center">
        <button type="button" class="btn btn-warning rounded-full" @click="closeAlert">확인</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const show = ref(false)
const message = ref('')
let timeoutId = null // 타이머 ID를 저장할 변수

const showAlert = (newMessage, duration = 5000) => {
  message.value = newMessage
  show.value = true

  // 이전 타이머가 있다면 취소
  if (timeoutId) {
    clearTimeout(timeoutId)
  }

  // 일정 시간 후에 자동으로 닫기 (기본값 5초)
  timeoutId = setTimeout(() => {
    closeAlert()
  }, duration)
}

const closeAlert = () => {
  show.value = false
  message.value = ''
  timeoutId = null // 타이머 ID 초기화
}

defineExpose({
  showAlert,
  closeAlert,
})
</script>
<style scoped>
.modal {
  width: 100%;
}
svg {
  width: 3rem;
  height: 3rem;
}
h3 {
  color: red;
}
.mb-4 {
  color: white;
}
.modal-box {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  max-width: 580px !important;
}
.modal-action {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 0;
}
</style>
