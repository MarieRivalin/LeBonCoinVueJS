<!-- eslint-disable no-unused-vars -->
<script setup>
import { RouterLink } from 'vue-router'
import { computed } from 'vue'

const props = defineProps({
  offerInfos: Object
})

//console.log(props.offerInfos.attributes.pictures.data[0].attributes.url)

const formatedDate = computed(() => {
  return props.offerInfos.attributes.publishedAt.split('T')[0].split('-').reverse().join('/')
})

const formatedPrice = computed(() => {
  const price = props.offerInfos.attributes.price

  const priceStr = price.toString()

  let newStr = ''

  for (let i = priceStr.length - 1; i >= 0; i--) {
    if (i === priceStr.length - 4 || i === priceStr.length - 7) {
      newStr = priceStr[i] + ' ' + newStr
    } else {
      newStr = priceStr[i] + newStr
    }
  }
  return newStr
})
</script>

<template>
  <RouterLink :to="{ name: 'offer', params: { id: offerInfos.id } }" class="offerCard">
    <div>
      <div class="ownerPart">
        <img
          :src="offerInfos.attributes.owner.data.attributes.avatar.data.attributes.url"
          alt=""
          class="avatar"
          v-if="offerInfos.attributes.owner.data.attributes.avatar.data"
        />
        <p>{{ offerInfos.attributes.owner.data.attributes.username }}</p>
      </div>
      <img
        v-if="
          offerInfos.attributes.pictures &&
          offerInfos.attributes.pictures.data &&
          offerInfos.attributes.pictures.data.length > 0
        "
        :src="offerInfos.attributes.pictures.data[0].attributes.url"
        :alt="offerInfos.title"
      />
      <p>{{ offerInfos.attributes.title }}</p>
      <p>{{ formatedPrice }} €</p>

      <div>
        <p class="date">{{ formatedDate }}</p>

        <font-awesome-icon :icon="['far', 'heart']" />
      </div>
    </div>
  </RouterLink>
</template>

<style scoped>
.offerCard {
  /* border: 1px solid red; */
  width: calc((100% - 60px) / 5);
}
.ownerPart {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}
.avatar {
  height: 30px;
  width: 30px;
  border-radius: 50%;
  object-fit: cover;
}
.ownerPart p {
  font-weight: bold;
  font-size: 14px;
}
.offerCard > div > img {
  width: 100%;
  height: 240px;
  object-fit: cover;
  border-radius: 15px;
  margin-bottom: 10px;
}
.offerCard > div > p {
  font-weight: bold;
  margin-bottom: 10px;
}
.offerCard > div > div {
  display: flex;
  justify-content: space-between;
  color: var(--dark-grey);
}
.date {
  font-size: 12px;
}
.date + svg {
  font-size: 18px;
}
</style>
