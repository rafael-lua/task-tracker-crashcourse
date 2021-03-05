<template>
  <div class="container">
    <Header title="Task Tracker" />
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
  </div>
</template>

<script>
import Header from "./components/Header"
import Tasks from "./components/Tasks"

export default {
  name: 'App',
  components: {
    Header,
    Tasks
  },
  data() {
    return {
      tasks: []
    }
  },
  methods: {
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: !task.reminder} : task);
      // Explanation: console.log({...this.tasks[0], reminder: false, reminder: true});
      // Basically, the spread "..." syntax will expand the "task" object literal to the new object. (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#syntax)
      // Then the repeated key declaration will set the "reminder" to a new value.
      // You can also just expand the arrow function with returns and make the change. Like: task.reminder = !t.reminder; return task;
    }  
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: "Doctors Appointment",
        day: "March 1st at 2:30pm",
        reminder: true,
      },
      {
        id: 2,
        text: "Meeting at School",
        day: "March 3rd at 1:30pm",
        reminder: true,
      },
      {
        id: 3,
        text: "Food Shopping",
        day: "March 3rd at 11:00am",
        reminder: false,
      }
    ];
  }
}
</script>

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
