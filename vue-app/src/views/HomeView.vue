<script setup>
import ProjectCard from '../components/ProjectCard.vue';
import { ref } from 'vue'
import bibimhakIcon from '../assets/icons/bibimhak.png'
import aocIcon from '../assets/icons/aoc.jpeg'
import gametheoryIcon from '../assets/icons/game-theory.png'
import soccerIcon from '../assets/icons/soccer.png'

const projects = ref([
  {
    id: 1,
    title: 'Bibimhak',
    description: 'A filesharing and community platform for ESL teachers in South Korea',
    projectUrl: 'https://bibimhak.com',
    imageUrl: bibimhakIcon,
  },
  {
    id: 2,
    title: 'Reddit Soccer Highlights',
    description: 'A Vue.js frontend for watching soccer highlights from Reddit',
    projectUrl: 'https://soccer-highlights-frontend.onrender.com',
    imageUrl: soccerIcon,
  },
  {
    id: 3,
    title: 'Evolutionary game theory simulator',
    description: 'A sandbox and visualizer in your browser to simulate various games from game theory and economics',
    projectUrl: 'https://liamb0t.github.io',
    imageUrl: gametheoryIcon,
  },
  {
    id: 4,
    title: 'Advent of Code 2023',
    description: 'Solutions to AoC 2023 in Typescript',
    projectUrl: '/aoc/2023',
    imageUrl: aocIcon,
  },
  {
    id: 5,
    title: 'Advent of Code 2022',
    description: 'Solutions to AoC 2022 in Python',
    projectUrl: '/aoc/2022',
    imageUrl: aocIcon,
  },
])
</script>

<template>
  <div class="projects">
    <h1>Some projects and stuff I've made</h1>
    <div class="project-grid">
      <ProjectCard v-for="project in projects" :key="project.id" :project="project"/>
    </div>
  </div>
</template>

<style scoped>
.projects {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px; /* Adjust the gap according to your design */
}
</style>