<script setup>
import { RouterLink } from 'vue-router';
import { computed } from 'vue';

const props = defineProps({
  post: {
    type: Object,
    required: true
  }
});

</script>

<template>
<div class="container">
    <RouterLink :to="{ name: 'post', params: { title: post.id }}">
        <h2 class="title">{{ post.title }}</h2>
    </RouterLink>
    <div class="content"
        v-html="post.content
        .split(`\n`)
        .slice(1, 5)
        .join('\n')">
    </div>
    <RouterLink class="open-post-btn" :to="{ name: 'post', params: { title: post.id }}">
        Continue Reading
    </RouterLink>
</div>
</template>

<style scoped>
.container {
  height: 100%; /* Set a fixed height for each card */
  transition: transform 0.3s ease;
  border-bottom: 2px solid gainsboro;
  padding-bottom: 2rem;
  margin-bottom: 2rem;
}
.title:hover {
    color: maroon;
}
img {
    width: 100%;
    height: 100%;
    object-fit: contain;
}
.title {
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: 1rem;
}
.content {
    color: var(--color-text);
    margin-bottom: 2rem
}
.open-post-btn {
    color: var(--color-text);
    padding: 0.5rem 1.5rem;
    border-radius: 30px;
    border: 1px solid maroon;
}
.open-post-btn:hover {
    color: white;
    background-color: maroon;
}
</style>