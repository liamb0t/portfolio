<script setup>
import PostCard from '../components/PostCard.vue';
import { ref, onMounted, onBeforeUnmount } from 'vue'

const posts = ref([
  {
    id: 'bibimhak',
    title: 'Bibimhak - creating a filesharing community for ESL teachers in South Korea',
    date: 'Tuesday, 19 December 2023',
    imageUrl: null,
  },
])

const isMounted = ref(false);

onMounted(() => {
  isMounted.value = true;
});

onBeforeUnmount(() => {
  isMounted.value = false;
})

</script>

<template>
  <transition name="fade">
    <div v-show="isMounted" class="projects">
      <h1>Blog</h1>
      <div class="project-grid">
        <PostCard v-for="post in posts" :key="post.id" :post="post"/>
      </div>
    </div>
  </transition>
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
/* transition code */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
.fade-enter-to, .fade-leave-from {
  opacity: 1;
}
</style>