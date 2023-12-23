<script setup>
import PostCard from '../components/PostCard.vue';
import { ref, onMounted } from 'vue';
import { marked } from 'marked';

const posts = ref([]);

onMounted(async () => {
 
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

</script>

<template>
  <div class="loader-container" v-show="posts.length === 0">
    <span class="loader"></span>
    <span class="loader-text">Loading</span>
  </div>
  <transition name="fade">
    <div v-show="posts" class="posts">
      <PostCard v-for="post in posts.reverse()" :key="post.id" :post="post"/>
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

/* loader */
.loader-container {
  height: 50vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column  ;
}
.loader {
  width: 10rem;
  height: 10rem;
  border: 2px solid #838383;
  border-radius: 50%;
  display: inline-block;
  position: relative;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
  margin-bottom: 2rem;
}
.loader::after,
.loader::before {
  content: '';  
  box-sizing: border-box;
  position: absolute;
  left: 0;
  top: 0;
  background: #FF3D00;
  width: 6px;
  height: 6px;
  border-radius: 50%;
}
.loader::before {
  left: auto;
  top: auto;
  right: 0;
  bottom: 0;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
} 
.loader-text {
  font-size: large;
  font-weight: 600;
}
</style>


