<script setup>
import { ref } from 'vue'
import MakeupItem from './components/MakeupItem.vue'

const makeupList = ref([
  { id: 1, name: 'Lipstick', brand: 'Maybelline', shade: 'Red Velvet', owned: true },
  { id: 2, name: 'Foundation', brand: 'Loreal', shade: 'Natural Beige', owned: false },
])

const newName = ref('')
const newBrand = ref('')
const newShade = ref('')
let nextId = 3

// CREATE
function addMakeup() {
  if (newName.value.trim() === '' || newBrand.value.trim() === '') {
    alert('Please enter name and brand')
    return
  }
  makeupList.value.push({
    id: nextId++,
    name: newName.value,
    brand: newBrand.value,
    shade: newShade.value,
    owned: false
  })
  newName.value = ''
  newBrand.value = ''
  newShade.value = ''
}

// UPDATE
function updateMakeup(updatedItem) {
  const index = makeupList.value.findIndex(m => m.id === updatedItem.id)
  if (index!== -1) {
    makeupList.value[index] = updatedItem
  }
}

// DELETE
function deleteMakeup(id) {
  makeupList.value = makeupList.value.filter(m => m.id!== id)
}
</script>

<template>
  <div class="container">
    <h1>Makeup Wishlist</h1>

    <!-- CREATE FORM -->
    <div class="add-makeup">
      <input v-model="newName" placeholder="Product Name" />
      <input v-model="newBrand" placeholder="Brand" />
      <input v-model="newShade" placeholder="Shade/Color" />
      <button @click="addMakeup">Add</button>
    </div>

    <!-- LIST -->
    <div v-if="makeupList.length === 0" class="empty">No makeup added yet</div>

    <MakeupItem
      v-for="item in makeupList"
      :key="item.id"
      :item="item"
      @update="updateMakeup"
      @delete="deleteMakeup"
    />
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 30px auto;
  font-family: Arial, sans-serif;
}
h1 {
  text-align: center;
  margin-bottom: 20px;
}
.add-makeup {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr auto;
  gap: 8px;
  margin-bottom: 20px;
}
.add-makeup input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.add-makeup button {
  padding: 8px 16px;
  background: #ec4899;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.empty {
  text-align: center;
  color: gray;
}
</style>