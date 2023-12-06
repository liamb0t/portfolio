<script setup>
import { marked } from "marked";
import { defineProps, watchEffect, ref, onMounted } from "vue";

const content = ref('')

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
});

onMounted(() => {
  const client = new XMLHttpRequest();
  const path = `/blog/${props.title}.md`
  client.open('GET', path);
  client.onreadystatechange = function() {
    console.log(client.responseText)
    console.log(client)
    content.value = marked(client.responseText)
  }
  client.send();
});

</script>

<template>
  <div v-html="content"></div>
</template>
