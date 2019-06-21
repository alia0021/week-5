<template>
  <div>
    <h1>Todo List</h1>
    <TodoForm @newTodo="addTodo">
      <h2 slot="title">Add a Todo</h2>
      <p slot="desc">Your todos are saved</p>
      <p></p>
    </TodoForm>
    <!-- slot needs a pair tag to con -->

    <Todo :todos="todoList" @removeTodo="appDeleteTodo"/>
  </div>
</template>
<script>
import Todo from "@/components/Todo.vue";
import TodoForm from "@/components/TodoForm.vue";
import axios from "axios";
export default {
  data() {
    return {
      todoList: ["Walk the dogs", "Go for a ride"]
    };
  },
  components: {
    Todo,
    TodoForm
  },
  methods: {
    appDeleteTodo(index) {
      this.todoList.splice(index, 1);
    },
    addTodo(todo) {
      this.todoList.push(todo);
      axios
        .put(
          "https://alia0021-vue-axios.firebaseio.com/data.json",
          this.todoList
        )
        .then(response => {
          console.log("Your data was saved status: " + response.status);
        }) //if the put is successfull then "then" will run if not "catch" will run
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>
<style scoped>
ul {
  list-style: none;
  width: 50%;
  margin: 0 auto;
}
ul li {
  border-bottom: 1px solid #acacac;
  padding: 10px 0;
  margin-bottom: 10px;
}
</style>