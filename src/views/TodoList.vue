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
      todoList: []
    };
  },
  components: {
    Todo,
    TodoForm
  },
  methods: {
    appDeleteTodo(index) {
      this.todoList.splice(index, 1);
      axios.put(
        "https://alia0021-vue-axios.firebaseio.com/data.json",
        this.todoList
      );
    },
    addTodo(todo) {
      this.todoList.push(todo);
      axios
        .put(
          "https://alia0021-vue-axios.firebaseio.com/data.json",
          this.todoList
        )
        // arrow function => replaces the regular function syntax
        .then(response => {
          console.log(response);
          console.log("Your data was saved status: " + response.status);
        }) //if the put is successfull then "then" will run if not "catch" will run
        .catch(error => {
          console.log(error);
        });
    }
  },
  created() {
    axios
      .get("https://alia0021-vue-axios.firebaseio.com/data.json")
      .then(response => {
        console.log(response.data);
        if (response.data) {
          this.todoList = response.data;
        }
      })
      .catch(error => {
        console.log("There was an error in getting data: " + error.response);
      });
  }
};
</script>
<style>
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