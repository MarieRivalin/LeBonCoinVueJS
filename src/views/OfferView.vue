<script setup>
import { onMounted, ref, computed } from 'vue'
import axios from 'axios'
import { useCycleList } from '@vueuse/core'

const props = defineProps({
  id: String
})

const offerInfos = ref(null)

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://site--strapileboncoin--2m8zk47gvydr.code.run/api/offers/${props.id}?populate[0]=pictures&populate[1]=owner.avatar`
    )
    console.log('Response offerview', data.data.attributes.owner.data.attributes.username)

    offerInfos.value = data.data
  } catch (error) {
    console.log('catch offerView >>>', error)
  }
})

console.log('id >>>', props.id)
const formatedDate = computed(() => {
  return offerInfos.value.attributes.publishedAt.split('T')[0].split('-').reverse().join('/')
})

const cycleList = computed(() => {
  if (offerInfos.value.attributes.pictures.data) {
    const { state, next, prev } = useCycleList(offerInfos.value.attributes.pictures.data)

    return { state, next, prev }
  } else {
    return {}
  }
})
</script>

<template>
  <main>
    <p v-if="offerInfos == null" class="container">Chargement en cours...</p>
    <div class="container" v-else>
      <div class="leftCol">
        <div class="caroussel">
          <font-awesome-icon
            :icon="['fas', 'angle-left']"
            @click="cycleList.prev()"
            v-if="offerInfos.attributes.pictures.data?.length > 1"
          />
          <img :src="cycleList.state.value.attributes.url" alt="" />
          <font-awesome-icon
            :icon="['fas', 'angle-right']"
            @click="cycleList.next()"
            v-if="offerInfos.attributes.pictures.data?.length > 1"
          />
        </div>

        <p class="titre">{{ offerInfos.attributes.title }}</p>
        <p class="prix">{{ offerInfos.attributes.price }} €</p>
        <p class="date">{{ formatedDate }}</p>

        <h2>Description</h2>
        <p>{{ offerInfos.attributes.description }}</p>

        <font-awesome-icon :icon="['fas', 'map-marker-alt']" />
        <span>Agon-Coutainville (50230)</span>
      </div>

      <div class="rightCol">
        <div class="owner">
          <div>
            <img
              :src="offerInfos.attributes.owner.data.attributes.avatar.data.attributes.url"
              alt=""
            />

            <p>{{ offerInfos.attributes.owner.data.attributes.username }}</p>
          </div>
          <p class="identity">
            <font-awesome-icon :icon="['fas', 'check-double']" />Pièce d’identité vérifiée
          </p>
          <p><font-awesome-icon :icon="['far', 'clock']" />Répond généralement en 1 heure</p>
        </div>

        <div class="btnPart"><button>Acheter</button> <button>Message</button></div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  min-height: calc(100vh - var(--header-height) - var(--footer-height));
}
.container {
  display: flex;
  gap: 20px;
}

.leftCol {
  width: 65%;
  /* border: 1px solid green; */
}
.leftCol img {
  width: 100%;
  height: 350px;
  object-fit: contain;
  margin-bottom: 40px;
}
.caroussel {
  display: flex;
  align-items: center;
}
.caroussel svg {
  font-size: 18px;
  cursor: pointer;
}
.titre {
  font-weight: bold;
  font-size: 24px;
  margin-bottom: 20px;
}
.prix {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}
.date {
  font-size: 12px;
  color: var(--dark-grey);
  margin-bottom: 50px;
}
h2 {
  border-top: 1px solid var(--grey-med);
  padding-top: 20px;
  margin-bottom: 20px 0;
  font-weight: bold;
  font-size: 18px;
}
h2 + p {
  padding-bottom: 50px;
  border-bottom: 1px solid var(--grey-med);
  margin-bottom: 20px;
}

/* RIGHT COL */
.rightCol {
  width: 35%;
  /* border: 1px solid red; */
  height: 365px;
  padding: 20px;
  box-shadow: 0 0 5px var(--grey-med);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.owner > div {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
.owner img {
  height: 65px;
  width: 65px;
  border-radius: 50%;
}
.owner > div p {
  font-size: 18px;
  font-weight: bold;
  text-transform: uppercase;
}
.identity {
  color: var(--brown);
  background-color: var(--beige);
  font-size: 12px;
  padding: 5px;
  border-radius: 10px;
  width: fit-content;
  margin-bottom: 20px;
}
.identity + p {
  font-size: 14px;
}
.btnPart {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
button {
  background-color: var(--orange);
  color: white;
  font-weight: bold;
  border-radius: 15px;
  padding: 15px 0;
  border: none;
  font-size: inherit;
}
button:last-child {
  background-color: var(--blue-dark);
}
</style>
