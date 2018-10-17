
<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div> <h4>Number of total tasks todo: {{ numberOfIncompleteTasks() }} </h4></div>
    
      <div v-for="task in tasks">
        <div v-bind:class="{strike: task.completed}">
          <h2 v-on:click="toggleTask(task)"> {{ task.text }} </h2>
        </div>
      </div>

  <div> <button v-on:click="deleteTask()">Delete Completed Tasks</button> </div>

<div>
  <h3>Create New Task</h3>
  Task: <input v-model="newTask.text">
  Id: <input v-model="newTask.id">
  <button v-on:click="addTask()">Add Task</button>
</div>

    </div>
</template>

<style>

body {
  background-color: white;
  color: lightblue;
}

.strike {
  text-decoration: line-through;
}
</style>

<script>

   var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Tasks Todo",
      tasks: [],

      newTask: {text: "", id: "", completed: false}


    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/tasks").then(function(response) {
      this.tasks = response.data;
    }.bind(this));
  },
  methods: {
    toggleTask: function(inputTask) {
        inputTask.completed = !inputTask.completed;
      },
    addTask: function() {
      if (this.newTask.text!== "") {
        this.tasks.push(this.newTask);
        this.newTask = {text: "", id: "", completed: false}; 
        };
       },   
    completeTask: function(inputTask) {
      var index = this.tasks.indexOf(inputTask);
      this.tasks.splice(index, 1);
      },
    numberOfIncompleteTasks: function() {
      var count = 0
      this.tasks.forEach(function(task) {
        if (!task.completed) {
          count ++;
        }
      });
      return count
    },
    deleteTask: function() {
      var incompleteTasks = [];
      for(var i = 0; i < this.tasks.length; i++) {
        var task = this.tasks[i];

        if (!task.completed) {
          incompleteTasks.push(task);
        }
      }
      this.tasks = incompleteTasks;
      }
  },
  computed: {}
};
</script>
