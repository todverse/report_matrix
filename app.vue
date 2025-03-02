<template>
  <canvas id='canv'></canvas>
  <div class="matrix-theme">
    <header class="bg-black border-b border-green-500 shadow fixed w-full z-10 top-0">
      <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8 flex justify-between items-center">
        <h1 class="text-3xl font-bold text-green-400 matrix-title">Выполненные задачи за февраль 2025 года</h1>
        <NuxtLink to="/" class="text-green-400 hover:text-green-300 transition-colors">
          <span class="matrix-code">[ Главная ]</span>
        </NuxtLink>
      </div>
    </header>
    <main class="pt-24 bg-black min-h-screen">
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <NuxtPage />
        </div>
      </div>
    </main>
    <footer class="bg-black border-t border-green-500 shadow mt-10">
      <div class="max-w-7xl mx-auto py-4 px-4 sm:px-6 lg:px-8 text-center text-green-500">
        <p class="matrix-text">© 2025 Все права защищены</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
  import { onMounted } from 'vue';
  onMounted(() => {
    const canvas = document.getElementById('canv');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    const ctx = canvas.getContext('2d');
    let cols = Math.floor(window.innerWidth / 20) + 1;
    let ypos = Array(cols).fill(0);
    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    function matrix () {
      const w = window.innerWidth;
      const h = window.innerHeight;
      if (canvas.width !== w) {
        canvas.width = w;
        cols = Math.floor(window.innerWidth / 20) + 1;
        ypos = Array(cols).fill(0);
      }
      if (canvas.height !== h) {
        canvas.height = h;
      }
      ctx.fillStyle = '#0001';
      ctx.fillRect(0, 0, w, h);
      ctx.fillStyle = '#0f0';
      ctx.font = '15pt monospace';
      ypos.forEach((y, ind) => {
        const text = String.fromCharCode(Math.random() * 128);
        const x = ind * 20;
        ctx.fillText(text, x, y);
        if (y > 100 + Math.random() * 10000) ypos[ind] = 0;
        else ypos[ind] = y + 20;
      });
    }
    setInterval(matrix, 50);
  })
</script>

<style>
html {
  scroll-behavior: smooth;
}

body {
  font-family: 'Courier New', monospace;
  background-color: #000;
  color: #33ff33;
  position: relative;
  overflow-x: hidden;
}

#canv {
  position: fixed;
  top: 0;
  left: 0;
}

body::before {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(rgba(0, 0, 0, 0.97), rgba(0, 0, 0, 0.97)), 
              url('https://i.imgur.com/8yJCwKN.gif');
  background-size: cover;
  opacity: 0.15;
  pointer-events: none;
  z-index: -1;
}

.matrix-theme {
  min-height: 100vh;
}

.matrix-title {
  text-shadow: 0 0 8px #33ff33;
  animation: flicker 3s infinite;
}

.matrix-text {
  text-shadow: 0 0 5px #33ff33;
}

.matrix-code {
  font-family: 'Courier New', monospace;
  position: relative;
}

.matrix-code::after {
  content: "";
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 100%;
  height: 1px;
  background-color: #33ff33;
  animation: typing 1.5s steps(20) infinite;
}

@keyframes flicker {
  0%, 19.999%, 22%, 62.999%, 64%, 64.999%, 70%, 100% {
    opacity: 1;
  }
  20%, 21.999%, 63%, 63.999%, 65%, 69.999% {
    opacity: 0.8;
  }
}

@keyframes typing {
  0% {
    width: 0;
  }
  100% {
    width: 100%;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>