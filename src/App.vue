<!--the template is the ouput, the html -->
<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" />
    <!--passing in a addTask function which will be declaired in script-->
    <!--using a conditional directive to show the form when clicking the button with v-if or v-show -->
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <!--we are passing a dynamic array, if it changes we want to be send into Tasks so we v-bind tasks to the tasks data -->
    <!--setting a delete method for the task and defining it in the script section-->
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</div>
</template>

<!--the script is the logic, javascript-->
<script>
  // importing the Header file
  import Header from './components/Header'

  //importing the tasks from component/Tasks file
  import Tasks from './components/Tasks'
  // import Task from './components/Task'

  //importing the form to add a task
  import AddTask from './components/AddTask'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  //function that return the object tasks
  data() {
    return {
      tasks: [
    {
      "id": 1,
      "text": "Meeting at school",
      "day": "March 3rd at 1:30pm",
      "reminder": false
    },
    {
      "id": 2,
      "text": "Doctor App.",
      "day": "March 1st at 1:30pm",
      "reminder": true
    }
      ],
      //we show the form component based on this true or false
      showAddTask: false
    }
  },
  methods: {
    //creating a function to be able to toggle the add task button(to open and close the form), setting the property of showAddTask to the opposite
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    addTask(task) {
      //in the new array we spread the old tasks and add the new task
      this.tasks = [...this.tasks, task]
      this.saveTasks()
    },
    deleteTask(id) {
      //adding a confirmation message if to delete
      if(confirm('Are you sure?')) {
        this.tasks = this.tasks.filter((task) => task.id !== id)
        this.saveTasks()
      }
    },
    toggleReminder(id) {
      //updating a task reminder from true to false or opposite, we map through the entire array,
      //we put an arrow function and for each task we check if the task.id === to id passed in and to give the opposite of the current task, if false to give us true(if not marked as reminder to mark it as reminder)
      this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: !task.reminder} : task)
      this.saveTasks()
    },
    saveTasks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsed);
    }
  },
  //creating a lifecycle method(check what it does)
  created() {
    //localStorage.getItem returns null if the key 'tasks' does not exist, so we check that storedTasks is different than null
    const storedTasks = localStorage.getItem('tasks');
    if (storedTasks !== null) {
      this.tasks = JSON.parse(storedTasks);
    }
  }
}
</script>

<!--the style, css-->
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
