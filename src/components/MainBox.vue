<template>
  <div class="main-box" >
    <SearchBox :items="tasks" @update="updateTask" @toggle="toggleTask" />
    <div v-for="task in tasks" :key="task.id" class="dolist">
      <TodoItem :item="task" @update="updateTask" @toggle="toggleTask" />
    </div>

    <!-- 添加任务按钮 -->
    
    <img src="@/assets/images/add.png" alt="Add Icon" class="add-icon">
    <button class="add-button" @click="showAddModal = true">添加事项</button>
  
    <!-- 添加任务模态框 -->
    <div v-if="showAddModal" class="modal-overlay" @click="cancelAddTask">
      <div class="modal" @click.stop>
        <h3>添加事项</h3>
        <input v-model="newTaskTitle" placeholder="请输入标题" class="edit-input" />
        <textarea v-model="newTaskContent" placeholder="请输入内容内容" class="edit-textarea"></textarea>
        <div class="modal-buttons">
          <button @click="addTask" class="save-button">保存</button>
          <button @click="cancelAddTask" class="cancel-button">取消</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import SearchBox from './SearchBox.vue';
import TodoItem from './TodoItem.vue';

const tasks = ref([
  { id: 1, title: '事件1', content: '明暗切换不会做', completed: false },
  { id: 2, title: '事件2', content: '重要的功能都做完了', completed: false },
  { id: 3, title: '事件3', content: '左滑右滑也没有做，下拉日历做了', completed: false },
  { id: 4, title: '事件4', content: '我就做了最重要的功能，能力有限', completed: false },
]);

const showAddModal = ref(false);
const newTaskTitle = ref('');
const newTaskContent = ref('');

// 更新任务
const updateTask = (updatedTask) => {
  const index = tasks.value.findIndex(task => task.id === updatedTask.id);
  if (index !== -1) {
    tasks.value[index] = { ...tasks.value[index], ...updatedTask };
  }
};
// 切换任务状态
const toggleTask = (id) => {
  const index = tasks.value.findIndex(task => task.id === id);
  if (index !== -1) {
    tasks.value[index].completed = !tasks.value[index].completed;
  }
};
// 添加任务
const addTask = () => {
  if (newTaskTitle.value.trim() && newTaskContent.value.trim()) {
    const newTask = {
      id: Date.now(),
      title: newTaskTitle.value,
      content: newTaskContent.value,
      completed: false,
    };
    tasks.value.push(newTask);
    newTaskTitle.value = '';
    newTaskContent.value = '';
    showAddModal.value = false;
  } else {
    alert('请填写标题和内容');
  }
};
// 取消添加任务
const cancelAddTask = () => {
  newTaskTitle.value = '';
  newTaskContent.value = '';
  showAddModal.value = false;
};
</script>

<style scoped>
.main-box {
  padding: 16px;
  max-width: 375px;
  margin: 0 auto;
  padding-top: 30px;
}

.dolist  {
  position: relative;
  right:60px;
  margin-top: 10px;
}

/* 添加任务按钮 */
.add-button {
  background-color: #fff;
  color: rgba(0, 122, 255, 1);
  padding: 10px 5px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 14px;
  position: fixed;
  left: 55px;
  font-size: 17px;
}
.add-icon {
  width: 20px; 
  height: auto;
  margin-right: 8px; 
  margin-top: 25px;
  position: fixed;
  right: 320px;
}

.add-button:hover {
  background-color: rgba(0, 122, 255, 1);
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
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
  height: 670px;
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
  background-color: #fff;
}

.cancel-button {
  background-color: #f0f0f0;
}

.cancel-button:hover {
  background-color: #e0e0e0;
}
</style>
