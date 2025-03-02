<template>
  <div class="matrix-container">
    <div class="mb-10">
      <p class="text-lg text-green-400 mb-6 matrix-text">
        Отчет о выполненных задачах за февраль 2025 года.
      </p>
      
      <div class="flex flex-wrap gap-4 mb-8">
        <NuxtLink 
          v-for="(project, index) in projects" 
          :key="index"
          :to="`/project/${project.id}`"
          class="px-4 py-2 bg-black border border-green-500 text-green-400 rounded-md hover:bg-green-900 hover:text-green-300 transition-colors transform hover:scale-105 duration-300 flex items-center matrix-button"
        >
          <span class="mr-2 text-xl">{{ project.icon }}</span>
          {{ project.name }}
        </NuxtLink>
      </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <div v-for="(project, index) in projects" :key="index" 
        class="matrix-card transform transition-all duration-500 hover:scale-105"
      >
        <NuxtLink :to="`/project/${project.id}`">
          <div class="bg-black border border-green-500 rounded-lg shadow-lg overflow-hidden hover:shadow-green-500/50 transition-shadow duration-300">
            <div class="px-6 py-4 bg-gradient-to-r from-black to-green-900 border-b border-green-500">
              <h2 class="text-2xl font-bold text-green-400 flex items-center matrix-title">
                <span class="text-3xl mr-2">{{ project.icon }}</span> {{ project.name }}
              </h2>
            </div>
            <div class="p-6">
              <div class="mb-4 flex justify-between items-center">
                <div class="flex items-center">
                  <img :src="project.image" alt="Project illustration" class="w-16 h-16 object-contain mr-4 matrix-image" />
                  <p class="text-green-400 italic">{{ project.description }}</p>
                </div>
              </div>
              <div class="progress-bar mb-6 bg-green-900 rounded-full h-2.5">
                <div class="bg-green-500 h-2.5 rounded-full matrix-progress" :style="{ width: `${(project.tasks.length / maxTasks) * 100}%` }"></div>
              </div>
              <p class="text-sm text-green-500 mb-4">Выполнено задач: {{ project.tasks.length }}</p>
              <div class="text-center mt-4">
                <span class="text-green-400 matrix-code">[ Подробнее ]</span>
              </div>
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

import { projects } from '~/helpers';

const maxTasks = computed(() => {
  return Math.max(...projects.map(p => p.tasks.length));
});
</script>

<style scoped>
.matrix-container {
  background-color: #000;
  color: #33ff33;
  font-family: 'Courier New', monospace;
  padding: 20px;
}

.matrix-text {
  text-shadow: 0 0 5px #33ff33;
  animation: pulse 2s infinite;
}

.matrix-title {
  text-shadow: 0 0 8px #33ff33;
}

.matrix-button {
  box-shadow: 0 0 10px #33ff33;
  transition: all 0.3s ease;
}

.matrix-button:hover {
  box-shadow: 0 0 20px #33ff33;
}

.matrix-card {
  position: relative;
  overflow: hidden;
}

.matrix-card::before {
  content: "";
  position: absolute;
  top: -100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, transparent, rgba(51, 255, 51, 0.1), transparent);
  animation: matrix-rain 3s linear infinite;
  pointer-events: none;
}

.matrix-progress {
  box-shadow: 0 0 10px #33ff33;
}

.matrix-image {
  filter: drop-shadow(0 0 5px #33ff33);
}

.matrix-code {
  position: relative;
  overflow: hidden;
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

@keyframes matrix-rain {
  0% {
    top: -100%;
  }
  100% {
    top: 100%;
  }
}

@keyframes pulse {
  0% {
    opacity: 0.7;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0.7;
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

.progress-bar {
  transition: width 1s ease-in-out;
}
</style>