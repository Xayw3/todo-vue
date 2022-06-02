<template>
  <h1 class="title">Tasks list</h1>
  <form class="form" @submit.prevent>
    <input
      class="form__input"
      type="text"
      placeholder="Enter new task"
      v-model="inputValue"
    />
    <button class="form__submit btn" @click="addTodo">Submit</button>
  </form>
  <div v-if="todos.length > 0">
    <div class="btn-wrapper">
      <button class="btn" @click="filterAllTasks">All</button>
      <button class="btn" @click="filterByCompletedTasks">Completed</button>
      <button class="btn" @click="filterByProgressTasks">In progress</button>
    </div>
    <div class="task-list" v-if="todosForShow === 0">
      <div class="task" v-for="(todo, index) in todos" :key="index">
        <label class="task-text" :class="[todo.done ? doneClass : '']">
          <input type="checkbox" v-model="todo.done" />
          {{ todo.text }}
        </label>
        <button class="task-btn btn" @click="removeTodo">Delete</button>
      </div>
    </div>
    <div class="task-list" v-if="todosForShow === 1">
      <div class="task" v-for="(todo, index) in inProgressTasks" :key="index">
        <label class="task-text" :class="[todo.done ? doneClass : '']">
          <input type="checkbox" v-model="todo.done" />
          {{ todo.text }}
        </label>
        <button class="task-btn btn" @click="removeTodo">Delete</button>
      </div>
    </div>
    <div class="task-list" v-if="todosForShow === 2">
      <div class="task" v-for="(todo, index) in completedTasks" :key="index">
        <label class="task-text" :class="[todo.done ? doneClass : '']">
          <input type="checkbox" v-model="todo.done" />
          {{ todo.text }}
        </label>
        <button class="task-btn btn" @click="removeTodo">Delete</button>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@import "src/assets/styles/main.scss";
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@400;700&display=swap");
</style>

<script lang="ts">
import { defineComponent } from "vue";
import Todo from "@/models/TodoModel";

export default defineComponent({
  name: "HomeView",
  data: () => ({
    todos: [] as Todo[],
    todosForShow: 0,
    completedTodos: [] as Todo[],
    isEmpty: false,
    inputValue: "",
    doneClass: "done",
  }),
  computed: {
    inProgressTasks() {
      return this.todos.filter((el) => el.done === true);
    },
    completedTasks() {
      return this.todos.filter((el) => el.done === false);
    },
  },
  methods: {
    addTodo() {
      this.todos.push({
        text: this.inputValue,
        done: false,
      });
      this.inputValue = "";
    },
    removeTodo(index: any) {
      this.todos.splice(index, 1);
    },
    filterByCompletedTasks() {
      this.todosForShow = 1;
    },
    filterByProgressTasks() {
      this.todosForShow = 2;
    },
    filterAllTasks() {
      this.todosForShow = 0;
    },
  },
});
</script>
