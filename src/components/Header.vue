<template>
  <header>
    <h1>{{ title }}</h1>
    <Button v-show="homePage" @btn-click="$emit('toggle-add-task')" :text="styleAddTask == true ? 'Close' : 'Add Task'" :color="styleAddTask == true ? 'red' : 'green'" />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: String, // Example with a object and default value => title: { type: String, default: "Hello world!"}
    styleAddTask: Boolean
  },
  components: {
    Button
  },
  emits: ["toggle-add-task"],
  computed: {
    homePage() {
      if(this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    }
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