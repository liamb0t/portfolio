<script setup>
import { marked } from "marked";
import { defineProps, ref, onMounted } from "vue";

const content = ref('');

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
});

onMounted(async () => {
  try {
    const response = await fetch(`blog/${props.title}.md`);

    const text = await response.text();
    content.value = marked(text);
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
  <div v-html="content"></div>
</template>
