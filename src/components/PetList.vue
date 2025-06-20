<template>
  <div>

  <h2 class="section-title">Add a New Pet</h2>

    <!-- form to add a new pet -->
    <form @submit.prevent="addNewPet">
      <input v-model="nameInput" placeholder="Pet Name" required />
      <input v-model="typeInput" placeholder="Type (e.g. Dog)" required />
      <label>
        <input type="checkbox" v-model="adoptedInput" />
        Adopted
      </label>
      <button type="submit">Add Pet</button>
    </form>

    <!-- showing the pet list -->
    <h2 class="section-title">Pet List</h2>
    <ul>
      <li v-for="(pet, i) in petList" :key="i">
        <strong v-if="editing !== i">{{ pet.name }}</strong>
        <input v-else v-model="nameEdit" />

        (
        <span v-if="editing !== i">{{ pet.type }}</span>
        <input v-else v-model="typeEdit" />
        ) -

        <span v-if="editing !== i" :style="{ color: pet.adopted ? 'green' : 'red' }">
          {{ pet.adopted ? 'Adopted' : 'Available' }}
        </span>

        <label v-else>
          <input type="checkbox" v-model="adoptedEdit" /> Adopted
        </label>

        <button v-if="editing !== i" @click="startEditing(i, pet)">Edit</button>
        <button v-else @click="confirmEdit(i)">Save</button>
        <button @click="removePet(i)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// starter pets
const petList = ref([
  { name: 'Buddy', type: 'Dog', adopted: true },
  { name: 'Mittens', type: 'Cat', adopted: false }
])

// form state for new pet
const nameInput = ref('')
const typeInput = ref('')
const adoptedInput = ref(false)

// add a pet to the list
function addNewPet() {
  if (!nameInput.value || !typeInput.value) return  // should never hit this with 'required' but oh well

  petList.value.push({
    name: nameInput.value,
    type: typeInput.value,
    adopted: adoptedInput.value
  })

  // reset the form fields
  nameInput.value = ''
  typeInput.value = ''
  adoptedInput.value = false
}

// editing state
const editing = ref(null)
const nameEdit = ref('')
const typeEdit = ref('')
const adoptedEdit = ref(false)

function startEditing(idx, petObj) {
  editing.value = idx
  nameEdit.value = petObj.name
  typeEdit.value = petObj.type
  adoptedEdit.value = petObj.adopted
}

function confirmEdit(idx) {
  petList.value[idx] = {
    name: nameEdit.value,
    type: typeEdit.value,
    adopted: adoptedEdit.value
  }
  editing.value = null
}

// deletes pet at given index
function removePet(idx) {
  petList.value.splice(idx, 1)
}
</script>

<style scoped>
form {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 25px;
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.05); /* soft shadow for a little depth */
}

input[type="text"],
input[type="checkbox"] {
  padding: 8px;
  font-size: 1rem;
}

input[type="text"] {
  flex: 1 1 200px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 8px 14px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s ease;
  /* maybe add disabled styles later */
}

button:hover {
  background-color: #45a049; /* a tiny bit darker for hover */
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #fff;
  margin-bottom: 10px;
  padding: 12px;
  border-radius: 8px;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
}

li span {
  font-weight: bold;
  /* could give this some margin if needed */
}

label {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.9rem;
  /* used to have margin-right here but removed it */
}
</style>
