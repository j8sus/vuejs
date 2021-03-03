<template>
  <div id="app">
    <h1>Todo Application</h1>
    <AddTodo @add-todo="addTodo" />
    <TodoList v-bind:todos="todos" @remove-todo="removeTodo" />
  </div>
</template>

<script>
import TodoList from "@/components/TodoList";
import AddTodo from "@/components/AddTodo";

export default {
  name: "App",
  data() {
    return {
      todos: [],
    };
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos")
      .then((response) => response.json())
      .then((json) => {
        this.todos = json;
      });
  },
  components: {
    TodoList,
    AddTodo,
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((t) => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  margin: 0 auto;
  width: 800px;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
