<template>

  <div v-if="showAddTask">
  <!-- v-if is conditional for rendering the block (https://v3.vuejs.org/guide/conditional.html) -->
  <AddTask @add-task="addTask" />
  </div>

  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
  <!-- 
    To understand :tasks="task" better: https://vuejs.org/v2/guide/syntax.html
    Basically, mustache {{}} sintaxe can't be used with html attributes, so you use binds instead.
  -->

  <!--
    (https://vuejs.org/v2/guide/events.html#Method-Event-Handlers)
    The deleteTask without () will pass the event object itself as the first paremeter, which in this case is the "id" coming from Tasks emit.
    With (), you can pass none or custom paremeters to the function.       
    Extra: (https://forum.vuejs.org/t/difference-when-calling-a-method-function-with-or-without-brackets/41764/4)
  -->
  
</template>

<script>
import Tasks from "../components/Tasks"
import AddTask from "../components/AddTask"

export default {
  name: "Home",
  props: {
    showAddTask: Boolean
  },
  components: {
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    // Methods have access to the data through the "this" keyword
    async deleteTask(id) {
      console.log(id);
      const res = await fetch(`api/tasks/${id}`, {
        method: "DELETE"
      });

      res.status === 200 ? this.tasks = this.tasks.filter((task) => task.id !== id) : alert("Error deleting task");

    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder};

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask)
      });
      const data = await res.json();

      this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task);
      // Explanation: console.log({...this.tasks[0], reminder: false, reminder: true});
      // Basically, the spread "..." syntax will expand the "task" object literal to the new object. (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#syntax)
      // Then the repeated key declaration will set the "reminder" to a new value.
      // You can also just expand the arrow function with returns and make the change. Like: task.reminder = !t.reminder; return task;
    },
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "post",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task)
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      // Backticks explanation: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals

      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    }
  },
  async created() {
    this.tasks = await this.fetchTasks();
  }
}
</script>