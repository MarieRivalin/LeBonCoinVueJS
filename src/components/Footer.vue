<!-- eslint-disable vue/multi-word-component-names -->
<script setup></script>

<template>
  <footer>
    <p class="container">
      Made by <a href="https://www.lereacteur.io/">Le Reacteur</a>
      with
      <a href="https://vuejs.org/"> Vue.js </a>
    </p>
  </footer>
</template>

<style scoped>
footer {
  height: var(--footer-height);
  background-color: var(--grey-light);
}
p {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
footer p a {
  margin: 0 5px;
}
a {
  font-weight: bold;
}
</style>
