<template>
  <div class="container">
    <img src="/public/images/gift.png" alt="Gift Box" class="gift-image" @click="handleClick" />

    <!-- Hiệu ứng lóa -->
  </div>
</template>

<script setup>
import { ref } from 'vue';
import party from 'party-js';

const showFlash = ref(false);
const emit = defineEmits(['updatePage']);

const handleClick = () => {
  if (showFlash.value) return; // tránh click nhiều lần

  showFlash.value = true;

  // Sau khi lóa xong thì chuyển trang
  emit('updatePage');
};

party.confetti(document.body, {
  count: party.variation.range(50, 100),
});
</script>

<style scoped>
.container {
  background-color: #ffe4ec; /* hồng nhạt */
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}

.gift-image {
  width: 200px;
  height: auto;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.gift-image:hover {
  transform: scale(1.05);
}

.flash-screen {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: white;
  animation: flash 1s forwards;
  z-index: 10;
}

@keyframes flash {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
