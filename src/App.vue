<script setup>
import party from 'party-js';
import happyBirthdaySong from '../src/assets/happy-birthday.mp3';
import { computed, onMounted, ref } from 'vue';
import BackGround2 from './components/BackGround2.vue';
import BackGround1 from './components/BackGround1.vue';
import StarSky from './components/StarSky.vue';
import BackGround3 from './components/BackGround3.vue';
import BackGround4 from './components/BackGround4.vue';
let timeoutId = null;
let isPage = ref(1);

const audio = new Audio(happyBirthdaySong);
audio.loop = true; // ✅ Tự động lặp
const playAudio = () => {
  audio.currentTime = 0; // Đặt lại thời gian về 0
  audio.play().catch(() => {
    window.addEventListener('click', playAudio, { once: true });
  });
};

onMounted(() => {
  playAudio();
  timeoutId = setInterval(() => {
    party.confetti(document.body, {
      count: party.variation.range(50, 100),
    });
  }, 1000);

  setTimeout(() => {
    clearTimeout(timeoutId);
    isPage.value = 2;
  }, 8000);
});

party.confetti(document.body, {
  count: party.variation.range(50, 100),
});

// party.sparkles(document.body, {
//   count: party.variation.range(20, 40),
//   speed: party.variation.range(1, 3),
// });

const currentComponent = computed(() => {
  switch (isPage.value) {
    case 1:
      return BackGround1;
    case 2:
      return BackGround2;
    case 3:
      return BackGround3;
    case 4:
      return BackGround4;
    default:
      return BackGround1; // fallback
  }
});
</script>

<template>
  <transition name="slide-left-right" mode="out-in">
    <!-- <StarSky /> -->
    <component :is="currentComponent" :key="`component_${isPage}`" @updatePage="isPage++" />
  </transition>
</template>

<style>
#app {
  width: 100% !important;
}

.slide-left-right-enter-active,
.slide-left-right-leave-active {
  transition: all 0.6s ease;
  /* position: absolute; */
  width: 100%;
}

.slide-left-right-enter-from {
  transform: translateX(-100%);
  opacity: 0;
}
</style>
