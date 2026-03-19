<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add']);
const title = ref('');
const author = ref('');
const genre = ref('');

const handleSubmit = () => {
  if (!title.value.trim() || !author.value.trim()) {
    alert('Заполните название и автора');
    return;
  }
  emit('add', { title: title.value, author: author.value, genre: genre.value || 'Другое' });
  title.value = ''; author.value = ''; genre.value = '';
};
</script>

<template>
  <div class="add-form">
    <h2>Добавить новую книгу</h2>
    <div class="form-row">
      <input v-model="title" type="text" placeholder="Название книги" />
      <input v-model="author" type="text" placeholder="Автор" />
      <select v-model="genre">
        <option value="">Жанр</option>
        <option>Роман</option>
        <option>Фантастика</option>
        <option>Детектив</option>
        <option>Научная</option>
        <option>Поэзия</option>
      </select>
      <button class="btn-add" @click="handleSubmit">+ Добавить</button>
    </div>
  </div>
</template>

<style scoped>
.add-form {
  background: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.08);
}
.add-form h2 {
  margin-bottom: 14px;
  color: #333;
  font-size: 1.1em;
}
.form-row {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}
.form-row input,
.form-row select {
  flex: 1;
  min-width: 140px;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 0.95em;
  outline: none;
  transition: border-color 0.2s;
}
.form-row input:focus,
.form-row select:focus {
  border-color: #667eea;
}
.btn-add {
  padding: 10px 20px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 0.95em;
  cursor: pointer;
  white-space: nowrap;
  transition: background 0.2s;
}
.btn-add:hover {
  background: #5a6fd6;
}
</style>