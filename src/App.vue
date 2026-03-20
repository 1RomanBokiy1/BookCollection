<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Моя коллекция</h1>

    <div class="row">
      <div class="col-md-6 offset-md-3">
        
        <form @submit.prevent="addBook" class="mb-4">

          <input v-model="newTitle" type="text" class="form-control mb-2" placeholder="Название" required>
          <input v-model="newDescription" type="text" class="form-control mb-2" placeholder="Описание">

          <input type="file" @change="handleFile" class="form-control mb-2">

          <button class="btn btn-primary w-100" type="submit">
            Добавить
          </button>
        </form>

        <!-- СОРТИРОВКА -->
        <button @click="sortBooks" class="btn btn-secondary mb-3 w-100">
          Сортировать A-Z
        </button>

      </div>
    </div>

    <div class="row">
      <div v-for="book in books" :key="book.id" class="col-md-4 mb-3">

        <div class="card h-100">
          <img :src="book.image" class="card-img-top" v-if="book.image">

          <div class="card-body">
            <h5>{{ book.title }}</h5>
            <p>{{ book.description }}</p>

            <button @click="removeBook(book.id)" class="btn btn-danger btn-sm">
              Удалить
            </button>
          </div>
        </div>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const books = ref([])

const newTitle = ref('')
const newDescription = ref('')
const newImage = ref('')

const addBook = () => {
  if (!newTitle.value) return

  books.value.push({
    id: Date.now(),
    title: newTitle.value,
    description: newDescription.value,
    image: newImage.value
  })

  newTitle.value = ''
  newDescription.value = ''
  newImage.value = ''
}

const removeBook = (id) => {
  books.value = books.value.filter(book => book.id !== id)
}

const sortBooks = () => {
  books.value.sort((a, b) => a.title.localeCompare(b.title))
}

const handleFile = (event) => {
  const file = event.target.files[0]
  readFile(file)
}

const handleDrop = (event) => {
  const file = event.dataTransfer.files[0]
  readFile(file)
}

const readFile = (file) => {
  if (!file) return

  const reader = new FileReader()
  reader.onload = (e) => {
    newImage.value = e.target.result
  }
  reader.readAsDataURL(file)
}
</script>