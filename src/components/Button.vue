<template>
  <!--adding an event to the button with v-on:click (shorter way for v-on is @) to listen to the DOM event and run js when triggered and set it to the function onClick() -->
  <!--adding style to the button with v-bind and we set the backgroung to the color prop, give text to button which we defined in Button file - text -->
  <button v-on:click="onClick()"
  :style="{ background: color }"
  class="btn">{{ text }}</button>
</template>

<script>
  //export the button to be recalled in Header file
  export default {
    name: 'Button',
    //adding the text and color properties
    props: {
      text: String,
      color: String
    },
    //defining methods and recall the onClick method used in button tag
    methods: {
      onClick() {
        this.$emit('toggle-add-task')
      }
    }
  }
</script>
