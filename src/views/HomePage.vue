<script setup>
import { ref, computed } from 'vue'
import SidebarLayout from '@/layouts/SidebarLayout.vue'
import CafeCard from '@/components/CafeCard.vue'
import BaseCheckbox from '@/components/base/BaseCheckbox.vue'
import BaseContainer from '@/components/base/BaseContainer.vue'
import BaseForm from '@/components/base/BaseForm.vue'
import BaseInput from '@/components/base/BaseInput.vue'

import { useFirestore, useCollection } from 'vuefire'

// These are helper methods we get from the Firestore library
import { collection, query, where, getDocs, onSnapshot } from "firebase/firestore"

const db = useFirestore()

/**
 * STUB: FIRESTORE WAY #1
 */
/* const cafeCollection = ref([])

// This is a query we can run on a specific collection we define and there's an optional parameter to even specify a conditional for what kind of documents within a collection you're looking for
const q = query(collection(db, "cafes"))

// This is where we call Firestore for all the docs within a collection which returns an array of our docs
const querySnapshot = await getDocs(q)

// This is an example of how you can loop through the array to log out the data inside of it
querySnapshot.forEach((doc) => {
  console.log(doc.id, " => ", doc.data())
  cafeCollection.value.push(doc.data())
}) */

/**
 * STUB: FIREBASE WAY #2 (LIVE UPDATES)
 */
/* const cafeCollection = ref([])

const q = query(collection(db, "cafes"))
const querySnapshot = await getDocs(q)

querySnapshot.forEach((doc) => {
  cafeCollection.value.push(doc.data())
})

onSnapshot(q, (querySnapshot) => {
  cafeCollection.value = []
  querySnapshot.forEach((doc) => {
    console.log(doc.id, "==>", doc.data())
    cafeCollection.value.push(doc.data())
  })
}) */

/**
 * REVIEW: VUEFIRE WAY
 */
const cafeCollection = useCollection(collection(db, 'cafes'))

const filterParams = ref({
  text: '',
  favorite: false,
})

const filteredCafes = computed(() => {
  return cafeCollection.value.filter((cafe) => {
    return (
      cafe.name.toLowerCase().includes(filterParams.value.text.toLowerCase()) &&
      (filterParams.value.favorite ? cafe.favorite : true)
    )
  })
})
</script>

<template>
  <SidebarLayout>
    <template v-slot:sidebar>
      <BaseContainer>
        <h2 class="mb-4">Filter</h2>
        <BaseForm>
          <BaseInput v-model="filterParams.text" label="Name" />
          <BaseCheckbox v-model="filterParams.favorite" label="Favorited" />
        </BaseForm>
      </BaseContainer>
    </template>

    <template v-slot:main>
      <BaseContainer>
        <h2 class="mb-4">Content</h2>
        <CafeCard v-for="cafe in filteredCafes" v-bind="cafe" :docId="cafe.id" :key="cafe.id" />
      </BaseContainer>
    </template>
  </SidebarLayout>
</template>

<style></style>
