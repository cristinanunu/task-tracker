<template>
  <!--with v-bind we set a reminder that check if is true or false, and we also keep the task class-->
  <!--using $emit to double click on the task and to mark it green as reminder instead of using a function-->
  <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>{{ task.text }}
      <!--adding delete icon, using$emit to delete a task with click event on icon -->
    <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
</div>
</template>

<script>
  export default {
    name: 'Task',
    props: {
      task: Object
    },
  }
</script>

<style scope>
  .fas {
    color: red;
  }
  .task {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
  }
  /* if the remind is set to true we want a border on the side of the task */
  .task.reminder {
    border-left: 5px solid green;
  }
  .task h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  </style>
