<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()

const props = defineProps(['title', 'sort', 'pricemin', 'pricemax', 'page', 'numOfPages'])

const changePage = (num) => {
  const queries = { ...props }

  delete queries.numOfPages

  queries.page = num

  router.push({ name: 'home', query: queries })
}
</script>
<template>
  <section>
    <font-awesome-icon
      :icon="['fas', 'angle-left']"
      @click="changePage(page - 1)"
      v-if="page > 1"
    />
    <font-awesome-icon :icon="['fas', 'angle-left']" v-else class="disabled" />
    <div>
      <p
        v-for="num in numOfPages"
        v-bind:key="num"
        :class="{
          selected: num === page
        }"
        @click="changePage(num)"
      >
        {{ num }}
      </p>
    </div>
    <font-awesome-icon
      :icon="['fas', 'angle-right']"
      @click="changePage(page + 1)"
      v-if="page < numOfPages"
    />
    <font-awesome-icon :icon="['fas', 'angle-right']" v-else class="disabled" />
  </section>
</template>
<style scoped>
section {
  display: flex;
  align-items: center;
  margin-top: 30px;
  gap: 10px;
}
section > div {
  display: flex;
}
p {
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
  cursor: pointer;
}
.selected {
  background-color: black;
  color: white;
}
svg {
  cursor: pointer;
}
.disabled {
  color: rgb(203, 198, 198);
  cursor: auto;
}
</style>
