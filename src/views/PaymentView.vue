<script setup>
import { onMounted, ref, computed } from 'vue'
import axios from 'axios'

const props = defineProps({ id: String })

const isLoading = ref(true)
const offerInfos = ref(null)
const option = ref('faceToFace')

const firstname = ref('')
const lastname = ref('')
const phone = ref('')

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://site--strapileboncoin--2m8zk47gvydr.code.run/api/offers/${props.id}?populate[0]=pictures`
    )
    console.log('data>>>', data)
    offerInfos.value = data.data
  } catch (error) {
    console.log(error)
  }
  isLoading.value = false
})

const total = computed(() => {
  let optionPrice = 0
  if (option.value === 'delivery') {
    optionPrice = 15.6
  }
  return 0.99 + optionPrice + offerInfos.value.attributes.price
})
</script>

<template>
  <main>
    <div class="container">
      <h1>Finalisez votre paiement</h1>
      <div>
        <div class="firstCol">
          <div>
            <h2>Informations personnelles</h2>
            <p>Une pièce d’identité vous sera demandée pour récupérer votre colis.</p>

            <label for="firstname">Prénom </label
            ><input
              type="text"
              id="firstname"
              name="firstname"
              placeholder="Prénom"
              v-model="firstname"
            />

            <label for="lastname">Nom </label
            ><input
              type="text"
              id="lastname"
              name="lastname"
              placeholder="Nom"
              v-model="lastname"
            />

            <label for="phone">Téléphone </label
            ><input type="text" id="phone" name="phone" placeholder="téléphone" v-model="phone" />

            <p>Recevoir un SMS pour l’arrivée de votre colis ou votre code de locker</p>
          </div>

          <p class="textbetween">
            Vous ne serez débité que lorsque le vendeur aura confirmé la disponibilité de la
            commande.
          </p>

          <div class="paymentInfo">
            <h2>Coordonnées bancaires</h2>

            <div id="card-element"></div>
            <div class="btnPart">
              <button>Payer</button>
            </div>

            <p>
              Paiement sécurisé Votre banque peut vous demander d’autoriser le paiement pour
              compléter votre achat.
            </p>
            <p>
              Vous êtes sur un serveur de paiement sécurisé par les normes ssl (https) et pcidss de
              nos partenaires bancaires. Vos données sont encryptées pour plus de sécurité.
            </p>
          </div>
        </div>

        <p v-if="isLoading">Chargement en cours...</p>
        <div v-else class="secondCol">
          <div>
            <div class="imgAndTitle">
              <img :src="offerInfos.attributes.pictures.data[0].attributes.url" alt="" />
              <!-- <p>{{ offerInfos.attributes.pictures.data[0].attributes.url }}</p> -->
              <h3>{{ offerInfos.attributes.title }}</h3>
            </div>

            <p class="price">{{ offerInfos.attributes.price }} €</p>
          </div>

          <div class="optionPart">
            <h3>Mode de paiement</h3>

            <div>
              <input
                type="radio"
                name="faceToFace"
                value="faceToFace"
                id="faceToFace"
                v-model="option"
              />
              <label for="faceToFace">
                <p>Remise en main propre</p>
                <p>Payez en ligne et récupérez votre achat en main</p>
                <p>propre lors de votre rendez-vous avec le vendeur</p></label
              >
            </div>

            <div>
              <input type="radio" name="delivery" value="delivery" id="delivery" v-model="option" />
              <label for="delivery">
                <p>Colissimo</p>
                <p>à votre domicile sous 2-3 jours</p>
              </label>

              <p class="price">15,60€</p>
            </div>

            <div>
              <h3>Protection leboncoin</h3>
              <p class="price">0,99 €</p>
            </div>

            <p>
              <font-awesome-icon :icon="['fas', 'check']" /><span
                >Votre argent est sécurisé et versé au bon moment</span
              >
            </p>
            <p>
              <font-awesome-icon :icon="['fas', 'check']" /><span
                >Notre service client dédié vous accompagne
              </span>
            </p>
          </div>

          <div>
            <h2>Total</h2>
            <p class="price">{{ total }} €</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: 40px 0;
  min-height: calc(100vh- var(--header-height) - var(--footer-height));
}
h1 {
  font-weight: bold;
  margin-bottom: 20px;
  font-size: 24px;
}
h2 {
  font-size: 18px;
  font-weight: bold;
  /* margin-bottom: 15px; */
}
h2 + p {
  font-size: 12px;
  line-height: 20px;
  margin-bottom: 15px;
}
h3 {
  font-weight: bold;
  margin-bottom: 10px;
}
.container > div {
  display: flex;
  align-items: flex-start;
  gap: 20px;
}
/* ----- first COL ---- */
.firstCol {
  flex: 1;
}

.firstCol > div {
  border-radius: 10px;
  padding: 20px 30px;
  box-shadow: 0 0 7px var(--grey-med);
  display: flex;
  flex-direction: column;
}
.firsrCol p {
  font-size: 12px;
  margin: 10px 0;
}
.textbetween {
  margin: 20px 0px;
  font-size: 12px;
  color: var(--black);
}
.firsrCol > p {
  margin: 20px 0;
}
.firsrCol label {
  margin-bottom: 10px;
}
.firstCol input {
  border: 1px solid var(--grey-med);
  border-radius: 15px;
  height: 45px;
  margin-bottom: 15px;
  padding-left: 10px;
}
#card-element {
  border: 1px solid var(--grey-med);
  border-radius: 15px;
  min-height: 45px;
  margin: 20px 0;
  padding: 15px;
}
.firstCol button {
  align-self: flex-start;
  background-color: var(--orange);
  color: white;
  padding: 7px 15px;

  border: none;
  border-radius: 15px;
  font-weight: bold;
  cursor: pointer;
}
.btnPart {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
.btnPart p {
  color: var(--orange);
  font-size: 12px;
  font-weight: bold;
}
.paymentInfo > p {
  font-size: 12px;
  margin: 5px 0;
}
/* ----- second COL ---- */
.secondCol {
  width: 355px;
  box-shadow: 0 0 7px var(--grey-med);
  border-radius: 10px;
}
.imgAndTitle {
  display: flex;
  gap: 10px;
  align-items: center;
  font-weight: bold;
}
.secondCol > div:not(:nth-child(2)) {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
}
.secondCol > div:first-child img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 5px;
}
.optionPart {
  border-top: 1px solid var(--grey-med);
  border-bottom: 1px solid var(--grey-med);
  padding: 15px;
  margin: 20px 0;
}
.optionPart > div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
  margin: 10px 0;
}
.optionPart label p:not(:first-child) {
  font-size: 12px;
  color: var(--dark-grey);
  line-height: 25px;
}
.optionPart > p {
  display: flex;
  gap: 15px;
  margin-bottom: 10px;
}
svg {
  color: green;
}
.price {
  font-weight: bold;
  color: var(--brown);
  font-size: 16px;
  margin-bottom: 0;
}
</style>
