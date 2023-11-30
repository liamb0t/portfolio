<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps({
  year: {
    required: true
  }
})

const advents = ref({
  2023: [
    {
      day: 1,
      title: 'Undefined',
    }
  ]
})
</script>

<template>
<div class="projects">
  <h1>Advent of Code - {{ year }}</h1>
  <RouterLink 
    v-for="day in advents[year]"
    to="{ name: 'advent', params: { day: day.day }}" 
    :key="day.day"
  >
    {{ day.title }}
  </RouterLink>
</div>
</template>

<style scoped>
.projects {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>