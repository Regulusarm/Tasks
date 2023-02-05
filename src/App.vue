<template>
  <div class="container">
    <h1>Task Manager</h1>
    <div class="search-container">
      <input type="text" v-model="searchTerm" placeholder="Search for a task" class="input">
      <button class="button" @click="searchTasks">Search</button>
      <button class="button" @click="clearSearch">Clear</button>
    </div>
    <div class="form-container">
      <form style="width: 100%" @submit.prevent="addTask">
        <input type="text" v-model="newTask" placeholder="Enter a new task" class="input">
        <button type="submit" class="button">Add</button>
      </form>
    </div>
      <h2>Tasks</h2>
    <div class="task-list-container">
      <ul class="task-list" style="width: 100%">
        <li v-for="task in tasks" :key="task.id" class="task-item" v-bind:draggable="true" v-bind:class="{'dragging': task.isDragging, 'over': task.isOver}" @dragstart="dragTask(task)" @drop="dropTask(task)" @dragover="dragOver(task)" @dragover.prevent @mouseover="taskMouseOver(task)" @mouseout="taskMouseOut(task)">
          {{ task.name }}
          <button @click="removeTask(task)" class="remove-button">Remove</button>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: '',
      searchTerm: '',
      initialTasks: [],
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        name: this.newTask,
        id: Date.now(),
        over:false
      });
      this.newTask = '';
    },
    removeTask(task) {
      this.tasks = this.tasks.filter((t) => t.id !== task.id);
    },
    dragTask(task) {
      task.isDragging = true;
      this.draggedTask = task;
    },
    dropTask(task) {
      task.isDragging = false;
      task.over = false;
      this.draggedTask.isDragging = false;
      let index = this.tasks.indexOf(this.draggedTask);
      this.tasks.splice(index, 1);
      index = this.tasks.indexOf(task);
      this.tasks.splice(index, 0, this.draggedTask);
      this.draggedTask = null;
    },
    dragOver(task){
      task.over = true;
    },
    taskMouseOver(task){
      task.isOver=true;
    },
    taskMouseOut(task){
      task.isOver=false;
    },
    searchTasks(){
      this.tasks = this.tasks.filter(task => task.name.includes(this.searchTerm));
    },
    clearSearch(){
      this.searchTerm = "";
      this.tasks = this.initialTasks;
    }
  },
  created() {
    this.initialTasks = [...this.tasks];
  },
};
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
html {
   height: 100%;
   width: 100%;
}

body {
   min-height: 100%;
   width: 100%;
   background-color:#f0f0f0;
}
.container {
  margin-top: 1em;
  padding: 20px 55px;
  border-radius: 4px;
  height: 100%;
}
.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.input {
  padding: 0.5em;
  margin: 0.5em;
  font-size: 1.2em;
  border-radius: 4px;
  border: none;
  width: 80%;
}

.button {
  padding: 0.5em 1em;
  margin: 0.5em;
  font-size: 1.2em;
  background-color: #00bfff;
  color: white;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  width: 20%;
}

.task-list-container {
  margin-top: 1em;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80%;
  margin: auto;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  align-items: center;
  padding: 0.5em;
  margin: 0.5em;
  border-radius: 4px;
  background-color: #ffffff;
}

.remove-button {
  margin-left: auto;
  padding: 0.5em 1em;
  background-color: #ff0000;
  color: white;
  border-radius: 4px;
  border: none;
  cursor: pointer;
}

.task-item:hover{
  background-color: #ffffff;
}

.dragging{
  opacity: 0.5;
  background-color: #ddd;
}
.over{
  background-color: #eee;
}
.search-container {
  display: flex;
  justify-content: center;
}
</style>
