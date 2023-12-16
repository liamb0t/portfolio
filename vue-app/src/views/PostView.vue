<script setup>
import { marked } from 'marked';
import { defineProps, ref, onMounted } from 'vue';

const content = ref('');

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
});

onMounted(async () => {
  try {
    const response = await fetch(`/blogs/${props.title}.md`);
    
    if (!response.ok) {
      throw new Error(`Failed to fetch Markdown content for ${props.title}`);
    }

    const markdown = await response.text();
    console.log(markdown)
    content.value = marked(markdown);
    console.log(content.value)
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
  <div v-html="content"></div>
</template>