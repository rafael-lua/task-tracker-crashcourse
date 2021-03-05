<template>
  <div v-for="task in tasks" :key="task.id">
    <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
    <!-- to use $emit here, you need to declared the emits array in the component script. -->
  </div>
  <!-- "When using v-for, vue expects a unique key for each iteration" - Explains the :key bind -->
</template>

<script>
import Task from "./Task";

export default {
  name: "Tasks",
  props: {
    tasks: Array
  },
  components: {
    Task
  },
  emits: ["delete-task", "toggle-reminder"]
}
</script>