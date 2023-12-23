<script setup>
import { RouterLink, RouterView } from 'vue-router'
import picture from './assets/icons/profile-pic.jpg'
</script>

<template>
  <div class="wrapper">
  <header>
    <RouterLink to="/">
      <div class="img-container">
        <img :src="picture" alt="">
      </div>
    </RouterLink>
    <div class="flex">
      <h1 class="name">LIAM MCLAUGHLIN</h1>
      <nav>
        <RouterLink to="/">Projects</RouterLink>
        <RouterLink :to="{ name: 'blog' }">Blog</RouterLink>
        <a href="https://github.com/liamb0t" target="_blank">Github</a>
        <RouterLink to="/contact">About</RouterLink>
      </nav>
    </div>
  </header>
  <RouterView />
</div>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
  margin-bottom: 2rem;
}
.img-container {
  position: relative;
  width: 10rem;
  height: 10rem;
  overflow: hidden;
  border-radius: 50%;
    
}
img {
  display: block;
  position: relative;
  width: 100%;
  height: auto;
  object-fit: cover;
  border-radius: 50%;
  margin-right: 4rem;
  transform: scale(1.33);
  top: -3rem;
}
nav {
  width: 100%;
  text-align: center;
  margin-top: 2rem;
}
.flex {
  display: flex;
  flex-direction: column;
  padding-left: 4rem;
}

.name {
  font-weight: 600;
  width: fit-content;
  border-bottom: 4px solid rgb(225 35 35);
}

nav a.router-link-exact-active {
  color: maroon;
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding-right: 2rem;
  font-weight: 500;
  font-size: 1.5rem;
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
  }

  nav {
    text-align: left;
    font-size: 1rem;
    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
