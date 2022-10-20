<template>
  <!--looping through the tasks by creating a div and we have a v-for directive, that is how to create a list or loop through something-->
  <!--with a v-for look we need a unique key, in order to identify the list or loop-->
<div :key="task.id" v-for="task in tasks">
  <!--calling toggle-reminder from Task file and emit-->
  <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
</div>
</template>

<script>
import Task from './Task'

  export default {
    //taking the tasks array as a prop
    name: 'Tasks',
    props: {
      tasks: Array
    },
    components: {
      Task
    },
    //events we are emmiting
    emits: ['delete-task', 'toggle-reminder']
  }
</script>
