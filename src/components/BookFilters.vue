<script setup>
defineProps(['filter', 'search', 'total', 'readCount']);
defineEmits(['update:filter', 'update:search']);
 
const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' },
  { value: 'favorite', label: '★ Избранные' },
];
</script>
 
<template>
  <div class="filters">
    <input
      class="search-input"
      :value="search"
      @input="$emit('update:search', $event.target.value)"
      type="text"
      placeholder="🔍 Поиск по названию или автору..."
    />
    <div class="filter-btns">
      <button
        v-for="opt in filterOptions" :key="opt.value"
        :class="['filter-btn', { active: filter === opt.value }]"
        @click="$emit('update:filter', opt.value)"
      >{{ opt.label }}</button>
    </div>
    <div class="stats">
      Всего: {{ total }} | Прочитано: {{ readCount }} | Осталось: {{ total - readCount }}
    </div>
  </div>
</template>
 
<style scoped>
.filters {
  background: white;
  padding: 16px 20px;
  border-radius: 10px;
  margin-bottom: 20px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.08);
  display: flex;
  flex-direction: column;
  gap: 12px;
}
 
.search-input {
  width: 100%;
  padding: 10px 14px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1em;
  outline: none;
  transition: border-color 0.2s;
}
.search-input:focus {
  border-color: #667eea;
}
 
.filter-btns {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
 
.filter-btn {
  padding: 7px 16px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9em;
  transition: all 0.2s;
}
.filter-btn:hover {
  background: #f0f0f0;
}
.filter-btn.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}
 
.stats {
  font-size: 0.88em;
  color: #888;
  padding-top: 8px;
  border-top: 1px solid #eee;
}
</style>