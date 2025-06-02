<template>
    <div class="word" :style="{ animationDelay: delay + 's' }">
      <span v-for="(letter, index) in text" :key="index" class="letter">
        <img
          v-for="i in 10"
          :key="i"
          class="star"
          src="@/assets/star.png"
          :style="{
            transform: getRandomTransform(),
            animationDelay: (i * 0.1) + 's'
          }"
        />
        <span class="letter-mask">{{ letter }}</span>
      </span>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  defineProps({
    text: String,
    delay: Number
  });
  
  const getRandomTransform = () => {
    const x = Math.random() * 200 - 100;
    const y = Math.random() * 200 - 100;
    return `translate(${x}px, ${y}px)`;
  };
  </script>
  
  <style scoped>
  .word {
    display: flex;
    justify-content: center;
    font-size: 60px;
    color: white;
    font-weight: bold;
    position: relative;
    animation: appear 0.5s ease-out forwards;
    opacity: 0;
  }
  
  @keyframes appear {
    to {
      opacity: 1;
    }
  }
  
  .letter {
    position: relative;
    margin: 0 8px;
  }
  
  .star {
    position: absolute;
    top: 0;
    left: 0;
    width: 6px;
    height: 6px;
    animation: flyin 1s forwards;
  }
  
  @keyframes flyin {
    from {
      transform: translate(0, 0) scale(0);
      opacity: 0;
    }
    to {
      transform: translate(0, 0) scale(1);
      opacity: 1;
    }
  }
  
  .letter-mask {
    position: relative;
    z-index: 2;
  }
  </style>
  