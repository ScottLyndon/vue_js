<!-- src/components/HelloWorld.vue -->
<template>
  <div class="todo-container">
    <h1>My To-Do List</h1>
    
    <!-- Input field to add a new task -->
    <div class="input-container">
      <input v-model="newTask" type="text" placeholder="Add a new task..." />
      <button @click="addTask">Add Task</button>
    </div>
    
    <!-- Task list with scrollbar -->
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        <div v-if="!task.isEditing" @dblclick="editTask(task)">
          {{ task.text }}
        </div>
        <input v-else v-model="task.text" @blur="finishEdit(task)" @keyup.enter="finishEdit(task)" />
        <div class="task-actions">
          <button @click="editTask(task)" v-if="!task.isEditing">Edit</button>
          <button @click="finishEdit(task)" v-if="task.isEditing">Save</button>
          <button @click="confirmDeleteTask(index)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [] // Empty list for initial tasks
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, isEditing: false });
        this.newTask = '';
      }
    },
    editTask(task) {
      task.isEditing = true; // Enable editing mode
    },
    finishEdit(task) {
      task.isEditing = false; // Disable editing mode
    },
    confirmDeleteTask(index) {
      // Add confirmation before deleting the task
      const confirmed = confirm('Are you sure you want to delete this task?');
      if (confirmed) {
        this.deleteTask(index);
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.todo-container {
  max-width: 500px;
  margin: 0 auto;
  margin-top: 50px;
  background: linear-gradient(90deg, #00F0FF 0%, #00FFE0 89.5%);
  padding: 20px;
  border-radius: 10px;
}

h1 {
  text-align: center;
  color: #333;
}

.input-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  margin-top: 20px
}

input[type="text"] {
  width: 70%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  background-color: #00AFFF;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #008ECC;
}

.task-list {
  list-style-type: none;
  padding: 0;
  max-height: 400px; /* Set max height for scrollbar */
  overflow-y: auto;  /* Add scrollbar when content exceeds height */
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #F0F8FF;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}

.task-actions button {
  margin-left: 10px;
}

input[type="text"]:focus {
  outline: none;
  border: 1px solid #00AFFF;
}

/* Style scrollbar for modern browsers */
.task-list::-webkit-scrollbar {
  width: 6px;
}

.task-list::-webkit-scrollbar-thumb {
  background-color: #00AFFF;
  border-radius: 5px;
}

.task-list::-webkit-scrollbar-track {
  background-color: #f1f1f1;
}
</style>
