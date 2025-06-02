<template>
  <div class="background-container">
    <!-- <div class="text-side">
      <h1 class="title">Happy Birthday Babe 🎉</h1>
      <p class="message">Chúc em luôn vui vẻ, hạnh phúc và gặp nhiều may mắn trong cuộc sống! 💖</p>
    </div> -->
    <div class="carousel-wrapper">
      <div class="text-side">
        <h1 class="title">{{ textImage[currentSlide].title }}</h1>
        <p class="description">{{ textImage[currentSlide].description }}</p>
      </div>
      <Carousel
        :items-to-show="1"
        :autoplay="2000"
        :wrap-around="true"
        v-model="currentSlide"
        class="carousel"
      >
        <Slide v-for="(item, index) in images" :key="index">
          <div class="image-container">
            <img :src="item.src" alt="" class="image" loading="lazy" />
            <p class="label">{{ item.date }}</p>
          </div>
        </Slide>
      </Carousel>
    </div>
  </div>
</template>

<script setup>
import { Carousel, Slide } from 'vue3-carousel';
import 'vue3-carousel/dist/carousel.css';
import { ref, watch } from 'vue';
import party from 'party-js';

const images = ref([
  { src: '/happy-birthday/images/slide1.jpg', date: '12-01-2024' },
  { src: '/happy-birthday/images/slide2.jpg', date: '12-01-2024' },
  { src: '/happy-birthday/images/slide4.jpg', date: '12-01-2024' },
  { src: '/happy-birthday/images/slide8.jpg', date: '12-01-2024' },
  { src: '/happy-birthday/images/slide9.jpg', date: '16-01-2025' },
  { src: '/happy-birthday/images/slide3.jpg', date: '08-02-2025' },
  { src: '/happy-birthday/images/slide5.jpg', date: '23-04-2025' },
  { src: '/happy-birthday/images/slide6.jpg', date: '23-04-2025' },
  { src: '/happy-birthday/images/slide7.jpg', date: '23-04-2025' },
]);
const currentSlide = ref(0);

const textImage = ref([
  {
    title: 'Biểu cảm gì zợ 🥴',
    description: 'Cute quá cơ 😚',
  },
  {
    title: 'Full cây hồng 🌸',
    description: 'Hoa màu hồng nữa mới chịu 🌷😆',
  },
  {
    title: 'Tập tành thành Sky~~ 🎧',
    description: 'Checkin cùng MV Lạc Trôi Sơn Tùng MTP 🎤✨',
  },
  {
    title: 'Nón lá cơ đấy 👒',
    description: 'Đúng chuẩn phụ nữ Việt Nam 🌟',
  },
  {
    title: 'Cháu ở nhà ăn ít lắm 🍜',
    description: 'Đi ăn húp đến cạn nước 🥤😂',
  },
  {
    title: 'Tạo dáng các kiểu 💃',
    description: 'Vẫn chê không được kiểu nào 🤷‍♀️😂',
  },
  {
    title: 'Yêu tổ quốc Việt Nam ❤️',
    description: 'ĐỘC LẬP - TỰ DO - HẠNH PHÚC ✊',
  },
  {
    title: 'Bé tập tô 🎨',
    description: 'Make color là chính 🌈🖌️',
  },
  {
    title: 'Coupon yêu nước 💌',
    description: 'Auto đẹp đôi 💑',
  },
]);

// const images = ref([
//   { src: `https://picsum.photos/seed/${Math.random()}/800/600`, date: '2025-06-01' },
//   { src: `https://picsum.photos/seed/${Math.random()}/800/600`, date: '2025-06-01' },
//   { src: `https://picsum.photos/seed/${Math.random()}/800/600`, date: '2025-06-01' },
// ]);

// 1. Tạo hình trái tim SVG (dạng <svg><path /></svg>)
const heartPath = document.createElementNS('http://www.w3.org/2000/svg', 'path');
heartPath.setAttribute(
  'd',
  'M316.722,29.761c66.852,0,121.053,54.202,121.053,121.041c0,110.478-218.893,257.212-218.893,257.212S0,266.569,0,150.801 C0,67.584,54.202,29.761,121.041,29.761c40.262,0,75.827,19.745,97.841,49.976C240.899,49.506,276.47,29.761,316.722,29.761z'
);
heartPath.setAttribute('fill', 'currentColor');

const heartShape = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
heartShape.setAttribute('viewBox', '0 0 512 512');
heartShape.setAttribute('height', '20');
heartShape.setAttribute('width', '20');
heartShape.appendChild(heartPath);

// 2. Tạo emitter tại một phần tử bất kỳ (ví dụ: targetElement là nút hoặc ảnh)
function emitHeartBurst(targetElement) {
  party.scene.current.createEmitter({
    emitterOptions: {
      loops: 1,
      useGravity: false,
      modules: [
        new party.ModuleBuilder()
          .drive('size')
          .by((t) => 0.5 + 0.3 * (Math.cos(t * 10) + 1)) // nhịp tim
          .build(),
        new party.ModuleBuilder()
          .drive('rotation')
          .by((t) => new party.Vector(0, 0, 100).scale(t))
          .relative()
          .build(),
      ],
    },
    emissionOptions: {
      rate: 0,
      bursts: [{ time: 0, count: party.variation.skew(80, 30) }],
      sourceSampler: party.sources.dynamicSource(targetElement),
      angle: party.variation.range(0, 360),
      initialSpeed: 400,
      initialColor: party.variation.gradientSample(
        party.Gradient.simple(party.Color.fromHex('#ffa68d'), party.Color.fromHex('#fd3a84'))
      ),
    },
    rendererOptions: {
      shapeFactory: () => heartShape.cloneNode(true), // 🛠 clone mỗi lần render
      applyLighting: undefined,
    },
  });
}

const emit = defineEmits(['updatePage']);
// emitHeartBurst(document.body);

watch(currentSlide, (newIndex) => {
  // emitHeartBurst(document.body);
  if (newIndex === images.value.length - 1) {
    setTimeout(() => {
      emit('updatePage');
    }, 1500);
  }
});
</script>

<style scoped>
.background-container {
  background-image: url('/images/bg2.jpg');
  background-size: cover;
  background-position: center;
  height: 100vh;
  color: #fff;
  text-align: center;
  font-family: 'Comic Sans MS', cursive;
  display: flex;
  justify-content: center; /* căn giữa theo chiều ngang */
  align-items: center;
}

.text-side {
  width: 40%;
  text-align: center;
}

.carousel-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50vh;
  border-radius: 12px;
  width: 46%;
}

.carousel {
  height: 50vh;
  border-radius: 12px;
  width: 60%;
}

.image-container {
  height: 50vh;
  position: relative;
  border-radius: 12px;
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 12px;
}

.label {
  position: absolute;
  bottom: 10px;
  right: 12px;
  background: rgba(0, 0, 0, 0.5);
  color: white;
  padding: 4px 8px;
  border-radius: 6px;
  font-size: 14px;
}

.title {
  color: #1e3a8a;
  font-weight: bold;
}

.description {
  color: #10b981;
  font-weight: normal;
}
</style>
