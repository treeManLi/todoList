<template>
  <div class="search-container">
    <div class="date-box">
      <div class="today">今天</div>
      <div class="date">4月18日 周六</div>
    </div>
    <div class="search-box">
      <input 
        type="text" 
        v-model="searchText" 
        @focus="showDropdown = true" 
        @input="Search" 
        placeholder="快速搜索"
        class="search-input"
      />
      <div v-if="showDropdown" class="dropdown" ref="dropdown" v-click-outside="closeDropdown">
        <TodoItem 
          v-for="item in filteredItems" 
          :key="item.id" 
          :item="item" 
          @update="updateTask"
          @toggle="toggleTask"
          @edit-mode="toggleEditing"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import TodoItem from './TodoItem.vue';
const props = defineProps({
  items: Array,
});

const emit = defineEmits(['update', 'toggle']);

const searchText = ref('');
const showDropdown = ref(false);
const dropdownRef = ref(null); 

let debounceTimer;

const Search = () => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(() => {}, 300);
};

const filteredItems = computed(() => {
  return props.items.filter(item => 
    item.title.toLowerCase().includes(searchText.value.toLowerCase())
  );
});

const updateTask = (updatedItem) => {
  emit('update', updatedItem);
};

const toggleTask = (id) => {
  emit('toggle', id);
  closeDropdown();
};

const toggleEditing = (isEditing) => {
  showDropdown.value = isEditing;
};

const closeDropdown = () => {
  showDropdown.value = false;
};


const useClickOutside = (elementRef, callback) => {
  const handleClickOutside = (event) => {
    if (elementRef.value && !elementRef.value.contains(event.target)) {
      callback();
    }
  };

  onMounted(() => {
    document.addEventListener('click', handleClickOutside);
  });

  onUnmounted(() => {
    document.removeEventListener('click', handleClickOutside);
  });
};

onMounted(() => {
  useClickOutside(dropdownRef, closeDropdown);
});
</script>

<style scoped>
.search-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.date-box {
  text-align: center;
  margin-bottom: 10px;
  font-size: 1.2rem;
  line-height: 1.5;
  margin-top: 20px;
}
.today{
opacity: 1;
font-size: 24px;
font-weight: 500;
line-height: 34.75px;
}
.date{
opacity: 1;
font-size: 14px;
font-weight: 500;
line-height: 20.27px;
}

.search-box {
  position: relative;
  width: 343px;
}

.search-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  border: none;
  border-radius: 20px;
  background: rgba(246, 246, 246, 1);
  font-size: 1rem;
  outline: none;
}

.dropdown {
  position: absolute;
  width: 100%;
  background: white;
  border: 1px solid #ddd;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 12;
  margin-top: 2px;
  padding: 8px 0;
}

.dropdown .todo-item {
  border: none;
}
</style>
