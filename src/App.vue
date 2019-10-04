<template>
  <div id="app">
    <Header />
    <!-- menangkap dari add todo -->
    <AddTodo v-on:add-todo="addTodo" />
    <!-- Ini namanya props -->
    <!-- Cara untuk pass data ke child component -->
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "./components/Todos";
import AddTodo from "./components/AddTodo";
import Header from "./components/layouts/Header";
import axios from "axios";

export default {
  name: "app",
  components: {
    Todos,
    Header,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(
          request => (this.todos = this.todos.filter(todo => todo.id !== id))
        )
        .catch(error => console.log(error));
      // hanya mereturn todo id yang sama ketika di tekan
      this.todos = this.todos.filter(todo => todo.id != id);
    },
    addTodo(newTodo) {
      // desturcturing
      const { title, completed } = newTodo;

      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed
        })
        .then(response => (this.todos = [...this.todos, response.data]))
        .catch(error => console.log(error));
    }
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(response => (this.todos = response.data))
      .catch(error => console.log(error));
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
