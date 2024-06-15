<template>
  <div class="todo-item">
    <label class="custom-checkbox">
      <input type="checkbox" :checked="item.completed" @change="onToggle" />
      <span class="checkmark"></span>
    </label>
    <div class="text-container">
      <span class="title">{{ item.title }}</span>
      <small class="content">{{ item.content }}</small>
    </div>
    <button class="edit-button" @click="startEditing"> <img src="@/assets/images/edit.png" alt="Edit Icon" class="edit-icon"></button>

  
    <div v-if="editing" class="modal-overlay" @click="cancelEditing">
      <div class="modal" @click.stop>
        <h3>编辑事项</h3>
        <input v-model="editTitle" placeholder="请输入标题" class="edit-input" />
        <textarea v-model="editContent" placeholder="请输入内容" class="edit-textarea"></textarea>
        <div class="modal-buttons">
          <button @click="saveChanges" class="save-button">保存</button>
          <button @click="cancelEditing" class="cancel-button">取消</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  item: Object,
});

const emit = defineEmits(['update', 'toggle', 'edit-mode']);

const editing = ref(false);
const editTitle = ref(props.item.title);
const editContent = ref(props.item.content);

const startEditing = () => {
  editing.value = true;
  emit('edit-mode', true); 
};

const saveChanges = () => {
  editing.value = false;
  emit('update', { ...props.item, title: editTitle.value, content: editContent.value });
  emit('edit-mode', false);
};

const cancelEditing = () => {
  editing.value = false;
  emit('edit-mode', false); 
};

const onToggle = () => {
  emit('toggle', props.item.id);
};


watch(() => props.item, (newItem) => {
  if (!editing.value) {
    editTitle.value = newItem.title;
    editContent.value = newItem.content;
  }
});
</script>

<style scoped>
.todo-item {
  width: 335px;
  height: 88px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 5px;
  border-bottom: 1px solid #ddd;
  position: relative;
}

.text-container {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  margin-left: 8px;
}

.title {
  font-size: 1rem;
  font-weight: bold;
}

.content {
  font-size: 0.875rem;
  color: grey;
  margin-top: 4px;
}

.edit-button {
  background-color: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.edit-icon {
  width: 24px; 
  height: auto;
}

.edit-button:hover {
  background-color: #e0e0e0;
}

.custom-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-right: 10px;
  user-select: none;
}

.custom-checkbox input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  position: relative;
  height: 20px;
  width: 20px;
  background-color: white;
  border: 2px solid rgba(0, 122, 255, 1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.checkmark::after {
  content: "";
  position: absolute;
  display: none;
}

.custom-checkbox input:checked ~ .checkmark {
  background-color: rgba(0, 122, 255, 1);
}

.custom-checkbox input:checked ~ .checkmark::after {
  display: block;
}

.custom-checkbox .checkmark::after {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
  width: 6px;
  height: 12px;
  border: solid white;
  border-width: 0 2px 2px 0;
}

/* Modal */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 375px;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  min-width: 300px;
  height: 700px;
}

.edit-input, .edit-textarea {
  width: 100%;
  margin-top: 8px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
  box-sizing: border-box;
}

.edit-textarea {
  height: 500px;
  resize: none;
}

.modal-buttons {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}

.save-button, .cancel-button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 8px;
}

.save-button {
  background-color: rgba(0, 122, 255, 1);
  color: white;
}

.save-button:hover {
  background-color: rgba(0, 122, 255, 1);
}

.cancel-button {
  background-color: #f0f0f0;
}

.cancel-button:hover {
  background-color: #e0e0e0;
}

</style>
