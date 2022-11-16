<template>
  <div class="home">
    <FilterNav :current="current" @filterChange="current = $event" />
    <div v-if="todos.length">
      <div v-for="todo in filteredTodos" :key="todo.id">
        <SingleTodo
          :todo="todo"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleTodo from "../components/SingleTodo.vue";
import FilterNav from "../components/FilterNave.vue";

export default {
  name: "HomeView",
  components: { SingleTodo, FilterNav },
  data() {
    return {
      todos: [],
      current: "all",
    };
  },
  mounted() {
    fetch("http://localhost:3000/todoList")
      .then((res) => res.json())
      .then((data) => (this.todos = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDelete(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
    },
    handleComplete(id) {
      let p = this.todos.find((todo) => {
        return todo.id === id;
      });
      p.complete = !p.complete;
    },
  },
  computed: {
    filteredTodos() {
      if (this.current === "completed") {
        return this.todos.filter((todo) => todo.complete);
      }
      if (this.current === "ongoing") {
        return this.todos.filter((todo) => !todo.complete);
      }
      return this.todos;
    },
  },
};
</script>
