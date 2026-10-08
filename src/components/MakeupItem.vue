<script setup>
import { ref } from 'vue'

const props = defineProps({ item: Object })
const emit = defineEmits(['delete', 'update'])

const isEditing = ref(false)
const editedName = ref(props.item.name)
const editedBrand = ref(props.item.brand)
const editedShade = ref(props.item.shade)

function updateItem() {
  if (editedName.value.trim() === '' || editedBrand.value.trim() === '') return
  emit('update', {
   ...props.item,
    name: editedName.value,
    brand: editedBrand.value,
    shade: editedShade.value
  })
  isEditing.value = false
}

function toggleOwned() {
  emit('update', {...props.item, owned:!props.item.owned })
}

function deleteItem() {
  emit('delete', props.item.id)
}

function startEditing() {
  editedName.value = props.item.name
  editedBrand.value = props.item.brand
  editedShade.value = props.item.shade
  isEditing.value = true
}
</script>

<template>
  <div class="makeup-item">
    <!-- VIEW MODE -->
    <div v-if="!isEditing" class="view-mode">
      <input type="checkbox" :checked="item.owned" @change="toggleOwned" />
      <div class="info" :class="{ owned: item.owned }">
        <strong>{{ item.name }}</strong>
        <span>{{ item.brand }}</span>
        <span v-if="item.shade">- {{ item.shade }}</span>
      </div>
      <button @click="startEditing">Edit</button>
      <button @click="deleteItem">Delete</button>
    </div>

    <!-- EDIT MODE -->
    <div v-else class="edit-mode">
      <input v-model="editedName" placeholder="Product Name" />
      <input v-model="editedBrand" placeholder="Brand" />
      <input v-model="editedShade" placeholder="Shade" />
      <button @click="updateItem">Save</button>
      <button @click="isEditing = false">Cancel</button>
    </div>
  </div>
</template>


