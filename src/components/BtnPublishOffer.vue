<script setup></script>
<template>
  <button>
    <font-awesome-icon :icon="['far', 'plus-square']" />
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
