<template>
  <div v-for="task in tasks" :key="task.id">
    <h3>{{ task.text }}</h3>
  </div>
  <!-- "When using v-for, vue expects a unique key for each iteration" - Explains the :key bind -->
</template>

<script>
export default {
  name: "Tasks",
  props: {
    tasks: Array
  }
}
</script>