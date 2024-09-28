<script setup>
import { onMounted, ref, inject, computed } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
import BtnPublishOffer from '@/components/BtnPublishOffer.vue'

const globalStore = inject('GlobalStore')
const router = useRouter()

const userInfos = ref(null)

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://site--strapileboncoin--2m8zk47gvydr.code.run/api/users/me?populate[0]=offers&populate[1]=offers.pictures&populate[2]=avatar`,
      {
        headers: {
          Authorization: `Bearer ${globalStore.userInfos.value.token}`
        }
      }
    )
    console.log('data>>>>', data)

    userInfos.value = data
  } catch (error) {
    console.log(error)
  }
})

const totalOffers = computed(() => {
  const numOfOffers = userInfos.value.offers.length

  if (numOfOffers == 1) {
    return '1 annonce'
  } else {
    return `${numOfOffers} annonces`
  }
})

const deleteOffer = async (id) => {
  try {
    const { data } = await axios.delete(
      `https://site--strapileboncoin--2m8zk47gvydr.code.run/api/offers/${id}`,
      {
        headers: {
          Authorization: `Bearer ${globalStore.userInfos.value.token}`
        }
      }
    )
    console.log('response>>>>', data)

    router.go()
  } catch (error) {
    console.log(error)
  }
}
</script>

<template>
  <main>
    <p v-if="!userInfos" class="container">Chargement en cours...</p>
    <div v-else class="container">
      <div class="userInfos">
        <img :src="userInfos.avatar.url" alt="" v-if="userInfos.avatar" />
        <div v-else class="firstLetter">
          <p>{{ userInfos.username[0] }}</p>
        </div>

        <div>
          <h1>{{ userInfos.username }}</h1>

          <p>{{ userInfos.email }}</p>
        </div>
      </div>

      <div class="noOffers" v-if="userInfos.offers.length === 0">
        <h3>Vous n'avez aucune annonce en ligne</h3>
        <BtnPublishOffer />
      </div>

      <div v-else>
        <h3>{{ totalOffers }}</h3>

        <div class="offersBloc">
          <div v-for="offer in userInfos.offers" :key="offer.id">
            <img :src="offer.pictures[0].url" alt="" />
            <h2>{{ offer.title }}</h2>
            <p>{{ offer.price }} €</p>
            <font-awesome-icon :icon="['fas', 'trash']" @click="deleteOffer(offer.id)" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  min-height: calc(100vh - var(--header-height) - var(--footer-height));
  padding-top: 40px;
}
h1 {
  font-weight: bold;
  font-size: 24px;
  margin-bottom: 10px;
}
h3 {
  font-weight: bold;
  font-size: 18px;
}
.userInfos {
  border-radius: 15px;
  padding: 15px;
  border: 1px solid var(--dark-grey);
  display: flex;
  gap: 10px;
  align-items: center;
  margin-bottom: 40px;
}
.firstLetter {
  background-color: var(--dark-grey);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  width: 100px;
  border-radius: 50%;
  font-size: 40px;
  color: white;
}
.offersBloc {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 20px;
}
.offersBloc > div {
  display: flex;
  align-items: center;
  box-shadow: 0 0 7px var(--grey-med);
  border-radius: 10px;
  padding: 15px;
  height: 150px;
}
.offersBloc img {
  height: 100%;
  aspect-ratio: 1 / 1;
  border-radius: 10px;
  margin-right: 20px;
}
.offersBloc h2 {
  font-weight: bold;
  flex: 2;
  font-size: 22px;
}
.offersBloc p {
  flex: 1;
  color: var(--brown);
  font-weight: bold;
  font-size: 18px;
}
svg {
  color: var(--orange);
}
.noOffers {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
</style>
