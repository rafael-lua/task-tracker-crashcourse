<template>
  <form @submit="onSubmit" action="" class="add-form">
  <!-- onSubmit is called without "()", so the first paremeter passed will be the submitted form event object. -->
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
      <!-- 
        v-model will properly bind data based on the type of the input field. 
        You need to pass the name of a declared data variable in the component.
        (https://v3.vuejs.org/guide/forms.html)

        // {{ text }}
        If you add the above line and type on the input, it will properly update as well in a 2 way fashion.
        So changes on the data variable are reflected in the model and changes on the input fiel are reflected on the variable.
      -->
    </div>

    <div class="form-control">
      <label>Day & Time</label>
      <input type="text" v-model="day" name="day" placeholder="Add Day & Time" />
    </div>

    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      text: "",
      day: "",
      reminder: false
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault(); // Stop submitting and refreshing the page
      
      if(!this.text) {
        alert("Task can't be blank.");
        return;
      }

      const newTask = {
        // id: Math.floor(Math.random() * 100000), // json-server adds an id automatically
        text: this.text,
        day: this.day,
        reminder: this.reminder
      }

      // Clean form
      this.text = "";
      this.day = "";
      this.reminder = false;

      this.$emit("add-task", newTask);

    }
  }
}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>