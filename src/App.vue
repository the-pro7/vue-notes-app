<script setup>
import { ref } from "vue"

const noteBody = ref("")
const error = ref("")
const showModal = ref(false)
const notes = ref([])


// Generate random background color
const generateRandomBackgroundColor = () => `hsl(${Math.floor(Math.random() * 360)}, 100%, 75%)`
// document.body.style.backgroundColor = generateRandomBackgroundColor()

const addNote = () => {

  if(!noteBody.value || noteBody.value.length < 10) {
    return error = "None existent value or text too short"
  }

  notes.value.push({
    id: crypto.randomUUID(),
    text: noteBody.value,
    date: new Date().toLocaleDateString("en-US"),
    color: generateRandomBackgroundColor()
  })

  showModal.value = false
  noteBody.value = ""
}
</script>

<template>
  <div class="wrapper">
    <div v-if="showModal" class="modal">
      <span @click="showModal = false" class="close-button">Close</span>
      <textarea cols="30" rows="10" v-model="noteBody"></textarea>
      <button @click="addNote" class="add-button">Add note</button>
    </div>
    <nav>
      <h1 class="app-logo">Notes App</h1>
      <button @click="showModal = true">+ Add note</button>
      <p v-if="error">{{ error }}</p>
    </nav>
    <div class="container">

      <div class="card" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.color}">
        <h4>{{ note.text }}</h4>
        <p>{{ note.date }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  font-family: sans-serif;
  position: relative;
  height: 100vh;
  width: 100vw;
}

.wrapper nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3em 4em;
}

nav button {
  /* width: 40px; */
  /* aspect-ratio: 1; */
  border-radius: .5rem;
  padding: .3em .5em;
  cursor: pointer;
  background-color: #000;
  color: #fff;
  font-size: 1.5em;
  border: 0;
  outline: 3px solid #000;
  outline-offset: 2px;
}

.container {
  padding-inline: 4em;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 1em;
}

.container .card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 130px;
  aspect-ratio: 1;
  border-radius: .5rem;
  padding: 1em;
  word-wrap: break-word;
  word-break: break-all;
}

.modal {
  width: fit-content;
  border-radius: .5rem;
  display: flex;
  flex-direction: column;
  gap: 1em;
  position: absolute;
  inset: 0;
  margin: auto;

  > * {
    border-radius: .3rem;
  }

  textarea {
    outline: 3px solid #000;
  outline-offset: 2px;
  }

  .add-button {
    border: 0;
    padding: .6em ;
    cursor: pointer;
    font-weight: 600;
    font-size: 1em;
    background-color: #000;
    color: #fff;
    outline: 3px solid #000;
  outline-offset: 2px;
  }

  .close-button {
    position: relative;
    top: 0;
    right: 0;
    cursor: pointer;
    background-color: rgba(255, 0, 0, 0.2);
    color: #ff0000;
    width: fit-content;
    padding: .4em;
    font-weight: bold;
  }
}
</style>