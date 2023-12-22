<script setup>
import PostCard from '../components/PostCard.vue';
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { marked } from 'marked';

const posts = ref([]);
const isMounted = ref(false);

onMounted(async () => {
  isMounted.value = true;

  try {
    const files = ['bibimhak.md', 'game-theory-visualizer.md']; 

    // Fetch content for each file and convert it to a post object
    const postPromises = files.map(async (file) => {
      const response = await fetch(`/blogs/${file}`);
      
      if (!response.ok) {
        throw new Error(`Failed to fetch Markdown content for ${file}`);
      }

      const markdown = await response.text();
      const titleMatch = markdown.match(/^#\s*(.+)/m);
      const title = titleMatch ? titleMatch[1] : 'Untitled'; // Use 'Untitled' if no title is found

      return {
        id: file.replace('.md', ''),
        title: title,
        date: 'Some default date', 
        content: marked(markdown),
      };
    });

    posts.value = await Promise.all(postPromises);
  } catch (error) {
    console.error(error);
  }
});

onBeforeUnmount(() => {
  isMounted.value = false;
});
</script>

<template>
  <transition name="fade">
    <div v-show="isMounted" class="posts">
      <PostCard v-for="post in posts" :key="post.id" :post="post"/>
    </div>
  </transition>
</template>

<style scoped>
.posts {
  max-width: 42rem;
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
