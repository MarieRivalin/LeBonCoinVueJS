<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <button>
    <RouterLink :to="{ name: 'publish' }">
      <font-awesome-icon :icon="['far', 'plus-square']" />
    </RouterLink>
    Déposer une annonce
  </button>
</template>

<style scoped>
button {
  background-color: var(--orange);
  color: white;
  padding: 7px 10px 5px 10px;
  border: none;
  border-radius: 10px;
  font-family: inherit;
  font-weight: bold;
  font-size: inherit;
}
button svg {
  margin-right: 5px;
}
span {
  margin-left: 10px;
}
</style>
