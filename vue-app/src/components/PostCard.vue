<script setup>
import { RouterLink } from 'vue-router';

defineProps({
   post: {
    type: Object,
    required: true
   }
})

</script>

<template>
    <RouterLink :to="{ name: 'post', params: { title: post.id }}">
        <div class="container">
            <p class="title">{{ post.title }}</p>
            <p class="content">{{ post.description }}</p>
        </div>
    </RouterLink>
</template>

<style scoped>
.container {
  height: 100%; /* Set a fixed height for each card */
  transition: transform 0.3s ease;
  
}
.container:hover {
    transform: scale(1.05);
}
img {
    width: 100%;
    height: 100%;
    object-fit: contain;
}
.title {
    font-weight: 600;
    color: var(--color-text)
}
.content {
    color: var(--color-text)
}
</style>