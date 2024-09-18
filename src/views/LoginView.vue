<!-- eslint-disable no-undef -->
<script setup>
import { RouterLink, useRouter } from 'vue-router'
import { ref, inject } from 'vue'
import axios from 'axios'

const email = ref('')
const password = ref('')
const errorMessage = ref('')
const isSubmitting = ref(false)
const displayPassword = ref(false)

const GlobalStore = inject('GlobalStore')
const router = useRouter()

const handleSubmit = async () => {
  console.log({ identifier: email.value, password: password.value })
  if (email.value && password.value) {
    isSubmitting.value = true
    try {
      const { data } = await axios.post(
        'https://site--strapileboncoin--2m8zk47gvydr.code.run/api/auth/local',
        { identifier: email.value, password: password.value }
      )
      console.log('response >>>>', data)
      GlobalStore.changeUserInfos({ username: data.user.username, token: data.jwt })

      $cookies.set('userInfos', { username: data.user.username, token: data.jwt })

      router.push({ name: 'home' })
    } catch (error) {
      console.log('error>>>>', error)
      errorMessage.value = 'Un problème est survenu, veuillez essayer à nouveau'
    }
    isSubmitting.value = false
  } else {
    errorMessage.value = 'Veuillez remplir tous les champs'
  }
}
</script>

<template>
  <main>
    <div class="container">
      <form @submit.prevent="handleSubmit">
        <div>
          <h1>Bonjour !</h1>
          <h2>Connectez-vous pour découvrir toutes nos fonctionnalités.</h2>
        </div>

        <label for="email">
          <span>Email <sup>*</sup></span>
          <input type="email" name="email" id="email" v-model="email" @input="errorMessage = ''" />
        </label>

        <label for="password">
          <span>Mot de passe <sup>*</sup></span>
          <div class="inputPassword">
            <input
              :type="displayPassword ? 'text' : 'password'"
              name="password"
              id="password"
              v-model="password"
              @input="errorMessage = ''"
            />

            <div>
              <font-awesome-icon
                :icon="['far', 'eye-slash']"
                v-if="!displayPassword"
                @click="displayPassword = !displayPassword"
              />
              <font-awesome-icon
                :icon="['far', 'eye']"
                v-else
                @click="displayPassword = !displayPassword"
              />
            </div>
          </div>
        </label>

        <p v-if="isSubmitting">Connexion en cours...</p>
        <button v-else>Se connecter <font-awesome-icon :icon="['fas', 'arrow-right']" /></button>

        <p v-if="errorMessage" class="textError">{{ errorMessage }}</p>

        <p>
          Envie de nous rejoindre ?
          <RouterLink :to="{ name: 'signup' }"> Créer un compte </RouterLink>
        </p>
      </form>
    </div>
  </main>
</template>

<style scoped>
main {
  height: calc(100vh - var(--header-height) - var(--footer-height));
}
.container {
  background-image: url('../assets/illustration.png');
  background-size: contain;
  background-position: bottom;
  background-repeat: no-repeat;
  display: flex;
  justify-content: center;
  align-items: center;
}
form {
  /* border: 1px solid black; */
  height: 490px;
  width: 480px;
  padding: 30px;
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0 0 7px var(--grey-med);
  border-radius: 15px;
}
label {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
input {
  border: 1px solid black;
  height: 45px;
  border-radius: 15px;
  padding-left: 10px;
}
button {
  background-color: var(--orange);
  color: white;
  font-size: 13px;
  border-radius: 15px;
  border: none;
  padding: 10px 0;
  height: 45px;
  font-weight: bold;
}
input:focus {
  outline: none;
}
.inputPassword {
  border: 1px solid black;
  display: flex;
  border-radius: 15px;
}
.inputPassword > div {
  border-left: 1px solid black;
  display: flex;
  /* justify-content: center; */
  align-items: center;
  width: 40px;
}
.inputPassword > input {
  flex: 1;
  border: none;
}
input:focus {
  outline: none;
}
button > svg {
  font-size: 14px;
}
h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 15px;
}
svg {
  margin-left: 10px;
}
p:last-child {
  text-align: center;
}
a {
  font-weight: bold;
  text-decoration: underline;
}
.textError {
  text-align: center;
  color: var(--orange);
}
</style>
