<template>
  <div class="sky">
    <div class="moon-crescent">
      <div class="moon-shadow"></div>
    </div>
    <div class="stars">
      <div v-for="n in 100" :key="n" class="star" :style="randomStyle()"></div>
    </div>
  </div>
</template>

<script setup>
function randomStyle() {
  const top = Math.random() * 100;
  const left = Math.random() * 100;
  const size = Math.random() * 2 + 1;
  const duration = Math.random() * 5 + 5;

  return {
    top: `${top}%`,
    left: `${left}%`,
    width: `${size}px`,
    height: `${size}px`,
    animationDuration: `${duration}s`,
  };
}
</script>

<style scoped>
.sky {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: radial-gradient(ellipse at bottom, #0b1a2d 0%, #000 100%);
  overflow: hidden;
}

.moon-crescent {
  position: absolute;
  top: 10%;
  right: 10%;
  width: 120px;
  height: 120px;
  background: #fffdd0; /* màu trắng ngà */
  border-radius: 50%;
  box-shadow: 0 0 20px 8px #ffffff80;
  overflow: hidden;
}

.moon-shadow {
  position: absolute;
  top: 0;
  left: 40px; /* thay đổi giá trị này để cong ít hay nhiều */
  width: 120px;
  height: 120px;
  background: #0b1a2d; /* trùng với màu nền bầu trời */
  border-radius: 50%;
}

.stars {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.star {
  position: absolute;
  background: white;
  border-radius: 50%;
  opacity: 0.8;
  animation: twinkle ease-in-out infinite;
}

@keyframes twinkle {
  0%,
  100% {
    opacity: 0.3;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.3);
  }
}
</style>
