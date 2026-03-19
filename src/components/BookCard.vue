<script setup>
defineProps(['book']);
defineEmits(['toggle', 'delete', 'favorite', 'rate']);
</script>
 
<template>
  <div :class="['book-card', { completed: book.completed, favorite: book.favorite }]">
    <div class="book-info">
      <h3>{{ book.title }} <span v-if="book.favorite" class="fav-star">★</span></h3>
      <p class="book-author">{{ book.author }}</p>
      <span class="book-genre">{{ book.genre }}</span>
    </div>
 
    <div class="book-actions">
      <div v-if="book.completed" class="rating">
        <span
          v-for="star in 5" :key="star"
          @click="$emit('rate', { id: book.id, rating: star })"
        >{{ star <= book.rating ? '★' : '☆' }}</span>
      </div>
 
      <button :class="['btn', 'btn-fav', { 'is-fav': book.favorite }]" @click="$emit('favorite', book.id)">
        {{ book.favorite ? '★' : '☆' }}
      </button>
 
      <button :class="['btn', book.completed ? 'btn-done' : 'btn-primary']" @click="$emit('toggle', book.id)">
        {{ book.completed ? '✓ Прочитано' : 'Отметить' }}
      </button>
 
      <button class="btn btn-danger" @click="$emit('delete', book.id)">✕</button>
    </div>
  </div>
</template>
 
<style scoped>
.book-card {
  background: white;
  border-radius: 10px;
  padding: 16px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 6px rgba(0,0,0,0.08);
  border-left: 4px solid transparent;
  transition: all 0.3s;
}
.book-card.completed {
  border-left-color: #4CAF50;
  background: #f6fbf6;
  opacity: 0.85;
}
.book-card.favorite {
  border-left-color: #FFD700;
}
 
.book-info {
  flex: 1;
}
.book-info h3 {
  margin-bottom: 4px;
  color: #333;
}
.fav-star {
  color: #FFD700;
}
.book-author {
  color: #666;
  font-size: 0.9em;
  margin-bottom: 6px;
}
.book-genre {
  background: #e8eaf6;
  color: #5c6bc0;
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 0.8em;
}
 
.book-actions {
  display: flex;
  align-items: center;
  gap: 8px;
}
 
.rating span {
  cursor: pointer;
  color: #FFD700;
  font-size: 22px;
  transition: transform 0.15s;
}
.rating span:hover {
  transform: scale(1.3);
}
 
.btn {
  padding: 7px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.88em;
  transition: background 0.2s;
}
.btn-fav {
  background: #fff8e1;
  color: #f9a825;
  font-size: 1.2em;
}
.btn-fav.is-fav {
  background: #FFD700;
  color: white;
}
.btn-primary {
  background: #667eea;
  color: white;
}
.btn-primary:hover { background: #5a6fd6; }
.btn-done {
  background: #4CAF50;
  color: white;
}
.btn-done:hover { background: #43a047; }
.btn-danger {
  background: #f44336;
  color: white;
}
.btn-danger:hover { background: #e53935; }
</style>