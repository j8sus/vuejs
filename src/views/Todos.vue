<template>
  <div>
    <h2>Todo Application</h2>
    <router-link to="/">Home</router-link>

    <AddTodo @add-todo="addTodo" />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="uncompleted">Uncompleted</option>
    </select>
    <br />
    <Loader v-if="loading" />
    <TodoList
      v-else-if="filteredTodos.length"
      v-bind:todos="filteredTodos"
      @remove-todo="removeTodo"
    />
    <p v-else>Empty!</p>
  </div>
</template>

<script>
import TodoList from "@/components/TodoList";
import AddTodo from "@/components/AddTodo";
import Loader from "@/components/Loader";

export default {
  name: "App",
  data() {
    return {
      todos: [],
      loading: true,
      filter: "all",
    };
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos")
      .then((response) => response.json())
      .then((json) => {
        this.todos = json.slice(0, 15);
        this.loading = false;
      });
  },
  // watch: {
  //   filter(value) {
  //     console.log(value);
  //   },
  // },
  computed: {
    filteredTodos() {
      if (this.filter === "completed") {
        return this.todos.filter((t) => t.completed);
      } else if (this.filter === "uncompleted") {
        return this.todos.filter((t) => !t.completed);
      } else {
        return this.todos;
      }
    },
  },
  components: {
    TodoList,
    AddTodo,
    Loader,
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
