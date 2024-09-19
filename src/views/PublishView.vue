<script setup>
import { ref, inject } from 'vue'
import axios from 'axios'

const GlobalStore = inject('GlobalStore')

const title = ref('')
const description = ref('')
const price = ref(null)
const pictures = ref(null)

const handleSubmit = async () => {
  try {
    const formData = new FormData()
    for (const key in pictures.value) {
      if (Object.hasOwnProperty.call(pictures.value, key)) {
        formData.append('files.pictures', pictures.value[key])
      }
    }
    const stringifiedInfos = JSON.stringify({
      title: title.value,
      description: description.value,
      price: price.value,
      ownner: GlobalStore.userInfos.value.id
    })

    formData.append('data', stringifiedInfos)

    const { data } = await axios.post(
      'https://site--strapileboncoin--2m8zk47gvydr.code.run/api/offers',
      formData,
      {
        headers: {
          'Content-Type': 'multipart/form-data',
          Authorization: `Bearer ${GlobalStore.userInfos.value.token}`
        }
      }
    )

    console.log('data>>>', data)
  } catch (error) {
    console.log(error)
  }
}
</script>

<template>
  <main>
    <div class="container">
      <h1>Déposer une annonce</h1>

      <form @submit.prevent="handleSubmit">
        <label for="title">Titre de l'annonce</label
        ><input type="text" name="title" id="title" v-model="title" />
        <p>Vous n'avez pas besoin de mentionner « Achat » ou « Vente » ici.</p>

        <label for="description">Description de l'annonce : </label
        ><textarea
          name="description"
          id="description"
          cols="30"
          rows="10"
          v-model="description"
        ></textarea>
        <p>
          Nous vous rappelons que la vente de contrefaçons est interdite. Nous vous invitons à
          ajouter tout élément permettant de prouver l’authenticité de votre article: numéro de
          série, facture, certificat, inscription de la marque sur l’article, emballage etc.
          Indiquez dans le texte de l’annonce si vous proposez un droit de rétractation à
          l’acheteur. En l’absence de toute mention, l’acheteur n’en bénéficiera pas et ne pourra
          pas demander le remboursement ou l’échange du bien ou service proposé
        </p>

        <label for="price">Votre prix de vente</label>
        <div>
          <input type="number" name="price" id="price" v-model="price" />
          <p>€</p>
        </div>

        <input
          type="file"
          name="pictures"
          id="pictures"
          multiple
          @input="(event) => (pictures = event.target.files)"
        />
        <button>Déposer mon annonce</button>
      </form>
    </div>
  </main>
</template>

<style scoped></style>
