<template>
  <div>
    <div v-if="openImg" class="fixed bg-black w-full h-full top-0 left-0 z-10">
      <button class="float-right p-2 mr-5" @click="() => {openImg = false}">X</button>
      <div class="w-full flex">
        <button class="w-[10%] m-auto" @click="() => {project.images[currentOpenImg - 1]? currentOpenImg--: currentOpenImg}">prev</button>
        <div class="w-auto  m-auto">
          <img class="h-[90vh]" :src="project.images[currentOpenImg]">
        </div>
        <button class="w-[10%] m-auto" @click="() => {project.images[currentOpenImg + 1]? currentOpenImg++: currentOpenImg}">next</button>
      </div>
    </div>
    <div :class="openImg? 'blur matrix-project-page': 'matrix-project-page'">
      <div v-if="project" class="mb-8">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-3xl font-bold text-green-400 flex items-center matrix-title">
            <span class="text-4xl mr-3">{{ project.icon }}</span> {{ project.name }}
          </h2>
          <div class="flex space-x-4">
            <NuxtLink 
              v-if="prevProject" 
              :to="`/project/${prevProject.id}`"
              class="px-4 py-2 bg-black border border-green-500 text-green-400 rounded-md hover:bg-green-900 hover:text-green-300 transition-colors flex items-center matrix-button"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
              </svg>
              Предыдущий
            </NuxtLink>
            <NuxtLink 
              v-if="nextProject" 
              :to="`/project/${nextProject.id}`"
              class="px-4 py-2 bg-black border border-green-500 text-green-400 rounded-md hover:bg-green-900 hover:text-green-300 transition-colors flex items-center matrix-button"
            >
              Следующий
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </NuxtLink>
          </div>
        </div>
        <div class="bg-black border border-green-500 rounded-lg shadow-lg overflow-hidden hover:shadow-green-500/50 transition-shadow duration-300 mb-8">
          <div class="p-6">
            <div class="flex flex-col md:flex-row gap-6 mb-8">
              <div class="md:w-1/3">
                <img :src="project.image" alt="Project illustration" class="w-full h-auto object-contain matrix-image mb-4" />
                <div class="image-upload-area border-2 border-dashed border-green-500 rounded-lg p-4 text-center h-64 flex flex-col items-center justify-center">
                  <p class="text-green-400 mb-2">Место для загрузки изображений</p>
                  <p class="text-green-500 text-sm">Добавьте скриншоты проекта</p>
                  <div class="matrix-code mt-4">[ Загрузить изображение ]</div>
                </div>
              </div>
              <div class="md:w-2/3">
                <p class="text-green-400 italic mb-6 text-lg">{{ project.description }}</p>
                <div class="progress-bar mb-6 bg-green-900 rounded-full h-2.5">
                  <div class="bg-green-500 h-2.5 rounded-full matrix-progress" :style="{ width: `${(project.tasks.length / maxTasks) * 100}%` }"></div>
                </div>
                <p class="text-sm text-green-500 mb-6">Выполнено задач: {{ project.tasks.length }}</p>
                
                <img class="image-upload-area border-2 border-dashed border-green-500 rounded-lg p-4 text-center w-full flex flex-col items-center justify-center mb-6" v-for="(im, index) in project.images" :src="im" @click="() => {openImg = true; currentOpenImg = index}">

                <div class="image-upload-area border-2 border-dashed border-green-500 rounded-lg p-4 text-center h-48 flex flex-col items-center justify-center mb-6">
                  <p class="text-green-400 mb-2">Место для загрузки диаграмм</p>
                  <p class="text-green-500 text-sm">Добавьте графики и диаграммы</p>
                  <div class="matrix-code mt-4">[ Загрузить диаграмму ]</div>
                </div>
              </div>
            </div>

            <h3 class="text-xl font-bold text-green-400 mb-4 matrix-subtitle">Список выполненных задач:</h3>
            <ul class="space-y-3 task-list">
              <li v-for="(task, taskIndex) in project.tasks" :key="taskIndex" 
                class="flex p-3 rounded-md border border-green-900 hover:border-green-500 transition-colors duration-300"
                :class="{'animate-pulse': hoveredTask === taskIndex}"
                @mouseenter="hoveredTask = taskIndex"
                @mouseleave="hoveredTask = null"
              >
                <span class="text-green-500 mr-3 flex-shrink-0 matrix-check">✓</span>
                <span class="task-text">{{ task }}</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="image-upload-area border-2 border-dashed border-green-500 rounded-lg p-4 text-center h-64 flex flex-col items-center justify-center">
            <p class="text-green-400 mb-2">Место для загрузки изображений</p>
            <p class="text-green-500 text-sm">Добавьте скриншоты результатов</p>
            <div class="matrix-code mt-4">[ Загрузить изображение ]</div>
          </div>
          <div class="image-upload-area border-2 border-dashed border-green-500 rounded-lg p-4 text-center h-64 flex flex-col items-center justify-center">
            <p class="text-green-400 mb-2">Место для загрузки изображений</p>
            <p class="text-green-500 text-sm">Добавьте скриншоты процесса</p>
            <div class="matrix-code mt-4">[ Загрузить изображение ]</div>
          </div>
        </div>

        <div class="text-center">
          <NuxtLink to="/" class="inline-block px-6 py-3 bg-black border border-green-500 text-green-400 rounded-md hover:bg-green-900 hover:text-green-300 transition-colors matrix-button">
            Вернуться на главную
          </NuxtLink>
        </div>
      </div>
      <div v-else class="text-center py-20">
        <p class="text-2xl text-green-500 matrix-text">Проект не найден</p>
        <NuxtLink to="/" class="inline-block mt-6 px-6 py-3 bg-black border border-green-500 text-green-400 rounded-md hover:bg-green-900 hover:text-green-300 transition-colors matrix-button">
          Вернуться на главную
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRoute } from 'vue-router';

import { projects } from '~/helpers';

const openImg = ref(false)
const currentOpenImg = ref(0)

const route = useRoute();
const projectId = computed(() => route.params.id);
const hoveredTask = ref(null);


const project = computed(() => {
  return projects.find(p => p.id === projectId.value) || null;
});

const projectIndex = computed(() => {
  return projects.findIndex(p => p.id === projectId.value);
});

const prevProject = computed(() => {
  return projectIndex.value > 0 ? projects[projectIndex.value - 1] : null;
});

const nextProject = computed(() => {
  return projectIndex.value < projects.length - 1 ? projects[projectIndex.value + 1] : null;
});

const maxTasks = computed(() => {
  return Math.max(...projects.map(p => p.tasks.length));
});

onMounted(() => {
  // Add matrix code animation to the page
  const codeRain = document.createElement('div');
  codeRain.className = 'matrix-code-rain';
  document.body.appendChild(codeRain);
});
</script>

<style scoped>
.matrix-project-page {
  position: relative;
}

.matrix-subtitle {
  text-shadow: 0 0 6px #33ff33;
}

.matrix-check {
  text-shadow: 0 0 5px #33ff33;
  animation: pulse 2s infinite;
}

.matrix-image {
  filter: drop-shadow(0 0 8px #33ff33);
  transition: all 0.3s ease;
}

.matrix-image:hover {
  filter: drop-shadow(0 0 15px #33ff33);
  transform: scale(1.02);
}

.matrix-button {
  box-shadow: 0 0 10px #33ff33;
  transition: all 0.3s ease;
}

.matrix-button:hover {
  box-shadow: 0 0 20px #33ff33;
}

.matrix-progress {
  box-shadow: 0 0 10px #33ff33;
}

.task-list li {
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.task-list li::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, transparent, rgba(51, 255, 51, 0.1), transparent);
  animation: matrix-scan 3s linear infinite;
  pointer-events: none;
  opacity: 0;
}

.task-list li:hover::before {
  opacity: 1;
}

.task-text {
  position: relative;
}

.image-upload-area {
  transition: all 0.3s ease;
  cursor: pointer;
}

.image-upload-area:hover {
  border-color: #33ff33;
  box-shadow: 0 0 15px rgba(51, 255, 51, 0.5);
}

@keyframes matrix-scan {
  0% {
    left: -100%;
  }
  100% {
    left: 100%;
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

.animate-pulse {
  animation: task-pulse 1.5s infinite;
}

@keyframes task-pulse {
  0% {
    background-color: transparent;
  }
  50% {
    background-color: rgba(51, 255, 51, 0.1);
  }
  100% {
    background-color: transparent;
  }
}
</style>