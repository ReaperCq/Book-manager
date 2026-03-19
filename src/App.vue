<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import AddBookForm from './components/AddBookForm.vue';
import BookFilters from './components/BookFilters.vue';
import BookCard from './components/BookCard.vue';

// Состояние
const books = ref([]);
const currentFilter = ref('all');
const searchQuery = ref('');

// Загрузка данных
onMounted(() => {
  const saved = localStorage.getItem('books-p14');
  if (saved) books.value = JSON.parse(saved);
});

// Сохранение данных
watch(books, (val) => {
  localStorage.setItem('books-p14', JSON.stringify(val));
}, { deep: true });

// Методы
const addBook = (bookData) => {
  books.value.push({
    id: Date.now(),
    ...bookData,
    completed: false,
    favorite: false,
    rating: 0,
  });
};

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id);
  if (book) {
    book.completed = !book.completed;
    if (!book.completed) book.rating = 0;
  }
};

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id);
  }
};

const favoriteBook = (id) => {
  const book = books.value.find(b => b.id === id);
  if (book) book.favorite = !book.favorite;
};

const rateBook = ({ id, rating }) => {
  const book = books.value.find(b => b.id === id);
  if (book && book.completed) book.rating = rating;
};

// Фильтрация
const filteredBooks = computed(() => {
  return books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed;
      if (currentFilter.value === 'read') return book.completed;
      if (currentFilter.value === 'favorite') return book.favorite;
      return true;
    })
    .filter(book => {
      const q = searchQuery.value.toLowerCase();
      return book.title.toLowerCase().includes(q) || book.author.toLowerCase().includes(q);
    });
});
</script>

<template>
  <div class="app">
    <header>
      <h1>📚 Менеджер книг</h1>
      <p>Управляй своей библиотекой</p>
    </header>

    <AddBookForm @add="addBook" />

    <BookFilters
      v-model:filter="currentFilter"
      v-model:search="searchQuery"
      :total="books.length"
      :readCount="books.filter(b => b.completed).length"
    />

    <div v-if="filteredBooks.length === 0" class="empty">
      <div class="emoji">📖</div>
      <p>{{ books.length === 0 ? 'Добавьте первую книгу!' : 'Ничего не найдено' }}</p>
    </div>

    <div v-else>
      <BookCard
        v-for="book in filteredBooks"
        :key="book.id"
        :book="book"
        @toggle="toggleBook"
        @delete="deleteBook"
        @favorite="favoriteBook"
        @rate="rateBook"
      />
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Segoe UI', Arial, sans-serif;
  background: #f0f2f5;
  line-height: 1.6;
}
.app {
  max-width: 820px;
  margin: 0 auto;
  padding: 20px;
}
header {
  text-align: center;
  padding: 24px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
  margin-bottom: 24px;
  box-shadow: 0 4px 12px rgba(102,126,234,0.3);
}
header h1 {
  font-size: 2.2em;
  margin-bottom: 4px;
}
header p {
  opacity: 0.85;
  font-size: 1em;
}
.empty {
  text-align: center;
  padding: 50px 20px;
  color: #aaa;
}
.empty .emoji {
  font-size: 3.5em;
  margin-bottom: 16px;
}
.empty p {
  font-size: 1.1em;
}
</style>