<!-- eslint-disable vue/multi-word-component-names -->
<!-- eslint-disable no-undef -->
<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { RouterLink, useRoute, useRouter } from 'vue-router'
import BtnPublishOffer from './BtnPublishOffer.vue'
import { inject, ref } from 'vue'

const route = useRoute()
const router = useRouter()

const search = ref('')

const GlobalStore = inject('GlobalStore')
console.log(GlobalStore.userInfos)

const DisconnectUser = () => {
  GlobalStore.changeUserInfos(null)
  $cookies.remove('userInfos')
  router.push({ name: 'home' })
}

const handleSubmit = () => {
  console.log('handleSubmit>>>>', search.value, route.query)
  const queries = { ...route.query }

  if (search.value) {
    queries.title = search.value
  } else {
    delete queries.title
  }
  queries.page = 1

  router.push({ name: 'home', query: queries })
}

const changeValue = (event) => {
  if (event.target.value === '') {
    const queries = { ...route.query }

    delete queries.title
    router.push({ name: 'home', query: queries })
  }
}
</script>

<template>
  <header>
    <div class="container">
      <div class="topPart">
        <RouterLink :to="{ name: 'home' }">
          <img src="../assets/logo.svg" alt="logo LeBonCoin" />
        </RouterLink>

        <div class="middlePart">
          <BtnPublishOffer />

          <form @submit.prevent="handleSubmit">
            <input
              type="text"
              name="search"
              id="search"
              placeholder="Rechercher sur leBonCoin"
              v-model="search"
              @input="changeValue"
            />
            <button><font-awesome-icon :icon="['fas', 'search']" /></button>
          </form>
        </div>

        <div class="connectionPart">
          <RouterLink :to="{ name: 'login' }" v-if="!GlobalStore.userInfos.value">
            <font-awesome-icon :icon="['far', 'user']" />
            <p>Se connecter</p>
          </RouterLink>

          <div v-else class="disconnectPart">
            <div>
              <RouterLink :to="{ name: 'profile' }">
                <font-awesome-icon :icon="['far', 'user']" />
                <p>{{ GlobalStore.userInfos.value.username }}</p></RouterLink
              >
            </div>
            <font-awesome-icon :icon="['fas', 'sign-out-alt']" @click="DisconnectUser" />
          </div>
        </div>
      </div>

      <div class="bottomPart">
        <span>Immobilier</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span> Véhicules</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Locations de vacances</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span> Emploi</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Mode</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Maison & Jardin</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Famille</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Electronique</span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Loisirs </span>
        <font-awesome-icon :icon="['fas', 'circle']" />

        <span>Autres</span>
      </div>
    </div>
  </header>
</template>
<style scoped>
header {
  height: var(--header-height);
  position: fixed;
  top: 0;
  width: 100%;
  background-color: white;
  border-bottom: var(--dark-grey) 1px solid;
}

.container > div {
  display: flex;
}
/* TOP PART */
.topPart {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}

.topPart > div:last-child {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.topPart p {
  font-size: 12px;
}
.middlePart {
  display: flex;
  align-items: center;
  gap: 20px;
}
.middlePart > form {
  display: flex;
  background-color: var(--grey-light);
  padding: 7px;
  border-radius: 10px;
  width: 300px;
  align-items: center;
}
input {
  width: 250px;
  background-color: inherit;
  border: none;
  flex: 1;
  background-color: var(--grey-light);
}
input:focus {
  outline: none;
}
.middlePart > form svg {
  background-color: var(--orange);
  padding: 8px;
  box-sizing: content-box;
  border-radius: 7px;
}
.middlePart > form button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}
.middlePart input::placeholder {
  color: var(--black);
  font-size: 16px;
}
.connectionPart > a {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  /* border: 1px solid red; */
  font-size: 12px;
}
.connectionPart svg {
  font-size: 18px;
}
img {
  width: 140px;
}
.disconnectPart {
  display: flex;
  align-items: center;
  gap: 20px;
}
.disconnectPart > div > a {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
/* BOTTOM PART */
.bottomPart {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.bottomPart svg {
  font-size: 3px;
}
.bottomPart span {
  font-size: 14px;
}
</style>
