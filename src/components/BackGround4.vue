<template>
  <div class="wrapper">
    <div class="card">
      <h1>🎉 Chúc Mừng Sinh Nhật Babe! 🎂</h1>
      <div class="line" v-for="(line, lineIndex) in visibleLines" :key="lineIndex">
        <span
          v-for="(word, wordIndex) in line.split(' ')"
          :key="wordIndex"
          class="word"
          :style="{ opacity: shownWords > getWordGlobalIndex(lineIndex, wordIndex) ? 1 : 0 }"
        >
          {{ word }}
        </span>
      </div>
      <img
        v-if="shownWords >= allWords.length"
        src="https://media.giphy.com/media/3oriO0OEd9QIDdllqo/giphy.gif"
        alt="cute"
        width="150"
      />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue';
import party from 'party-js';

const message = `
Chúc em tuổi mới thật nhiều niềm vui, sức khoẻ và luôn thành công nhé! 🎉
Đặc biệt là thật nhiều sức khỏe nhé em. Tuổi mới chỉ tiêu cố gắng tăng vài chục cân nữa 💪🍗.
Mong rằng món quà này mang lại cho em nhiều niềm vui và hạnh phúc như những gì anh mang lại cho em ❤️🎁
Bớt cãi nữa nhé hihi 😜💖
`;

setTimeout(() => {
  party.confetti(document.body, {
    count: party.variation.range(50, 100),
  });
}, 500);

const lines = message.trim().split('\n'); // chia theo dòng
const allWords = lines.flatMap((line) => line.trim().split(' ')); // dùng cho tổng số từ
const shownWords = ref(0);

// Hiện từng từ một
onMounted(() => {
  const interval = setInterval(() => {
    if (shownWords.value < allWords.length) {
      shownWords.value++;
    } else {
      clearInterval(interval);
    }
  }, 250);
});

// Tính số từ của từng dòng để xác định dòng nào đã hiện đủ
const visibleLines = computed(() => {
  const result = [];
  let wordCount = 0;
  for (let line of lines) {
    const lineWords = line.trim().split(' ');
    const currentShown = shownWords.value - wordCount;
    if (currentShown > 0) {
      result.push(line);
    } else {
      break;
    }
    wordCount += lineWords.length;
  }
  return result;
});

function getWordGlobalIndex(lineIndex, wordIndex) {
  let count = 0;
  for (let i = 0; i < lineIndex; i++) {
    count += lines[i].trim().split(' ').length;
  }
  return count + wordIndex;
}

party.confetti(document.body, {
  count: party.variation.range(50, 100),
});
</script>

<style scoped>
.wrapper {
  background: linear-gradient(to bottom right, #ffb6c1, #f8a5c2);
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'Pacifico', cursive;
}

.card {
  background: white;
  padding: 30px;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
  z-index: 2;
  max-width: 800px;
  font-family: 'Pacifico', cursive;
}

.card h1 {
  font-family: 'Pacifico', cursive;
  font-size: 2.5rem;
  color: #e84393;
  margin-bottom: 20px;
}

.line {
  margin-bottom: 12px;
  font-family: 'Pacifico', cursive;
}

.word {
  display: inline-block;
  margin-right: 5px;
  transition: opacity 0.3s ease;
  opacity: 0;
  font-family: 'Pacifico', cursive;
}
</style>
