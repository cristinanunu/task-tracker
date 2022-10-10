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
      tasks: [],
      //we show the form component based on this true or false
      showAddTask: false
    }
  },
  //the filter method takes in a function, it will loop through and give back task.id that is different than the id passed at deleteTask,
  //but we are not persisting the delete because not working with api and external server
  methods: {
    //creating a function to be able to toggle the add task button(to open and close the form), setting the property of showAddTask to the opposite
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    //to add a task we set it to an array where we spread around the current tasks and ad a new one into it
    addTask(task) {
      this.tasks = [...this.tasks, task]
    },
    deleteTask(id) {
      //adding a confirmation messageif to delete
      if(confirm('Are you sure?')) {
        this.tasks = this.tasks.filter((task) => task.id !== id)
      }
    },
    toggleReminder(id) {
      //updating a task reminder from true to false or opposite, we map through the entire array,
      //we put an arrow function and fo each task we check if the task.id === to id passed in and to give the opposite of the current task, if false to give us true(if not marked as reminder to mark it as reminder)
      this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: !task.reminder } : task
      )
    },
    //adding a json server to run on port 5000 to and watch the db.json file(the fake api where we store the data-tasks), to run this do npm run backend
    //fetching data from backend
    async fetchTasks() {
      const res = await fetch('http://localhost:5000/tasks')
      const data = await res.json()
      return data
    },
    //fetching a specific task if we want to update it
    async fetchTasks(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`)
      const data = await res.json()
      return data
    },
  },
  //creating a lifecycle method(check what it does) then create a Tasks component to display the tasks
  async created() {
    this.tasks = await this.fetchTasks()
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
