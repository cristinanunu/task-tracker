<template>
  <header>
    <h1>{{ title }}</h1>
    <!--after registering the Button component in the script we add it here to display, with cap. B because is a component not just a button tag-->
    <Button @toggle-add-task="$emit('toggle-add-task')" text="Add task" color="green" />
    <!--if we want to have multiple buttons we just duplicate the one above-->
  </header>
</template>

<script>
// importing Button file
import Button from './Button'


  export default {
    name: 'Header',
    props: {
      title: String,
    },
    //we then have to register the Button as a component here
    components: {
      Button
    }
  }
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>
