<script setup lang="ts">
import type { Todo } from "@/models/todos";
import { ref, computed } from "vue";

let id = 0;
const hideCompleted = ref(false);
const newTodo = ref("");
const todos = ref<Todo[]>([]);
const filteredTodos = computed(() => {
  if (hideCompleted.value) {
    return todos.value.filter((t) => !t.isDone);
  } else {
    return todos.value.map((t) => ({ ...t }));
  }
});

const toggleHideCompleted = () => {
  hideCompleted.value = !hideCompleted.value;
};

const deleteCompleted = () => {
  todos.value = todos.value.filter((t) => !t.isDone);
};

const addTodo = () => {
  todos.value.push({ id, text: newTodo.value, isDone: false });
  newTodo.value = "";
  id++;
};

const deleteTodo = (id: number) => {
  todos.value = todos.value.filter((t) => t.id !== id);
};

const toggleTodo = (id: number) => {
  todos.value = todos.value.map((t) => {
    if (t.id === id) {
      return { ...t, isDone: !t.isDone };
    } else return t;
  });
};
</script>

<template>
  <div :class="'button-group'">
    <button v-if="hideCompleted" @click="toggleHideCompleted">Show all</button>
    <button v-else @click="toggleHideCompleted">Hide completed</button>

    <button @click="deleteCompleted">remove completed</button>
  </div>
  <div :class="'todo-list'">
    <ul>
      <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        :class="todo.isDone && 'done'"
        @click="toggleTodo(todo.id)"
      >
        {{ todo.text }}
        <button :class="'delete'" @click="deleteTodo(todo.id)">X</button>
      </li>
    </ul>
    <input type="text" placeholder="New todo..." v-model="newTodo" v-on:keyup.enter="addTodo" />
  </div>
</template>

<style scoped>
.todo-list {
  width: 100%;
  max-width: 500px;
}

.button-group {
  display: flex;
  gap: 10px;
}

ul {
  list-style-type: "- ";
  padding-left: 10px;
}

li {
  width: 100%;
  padding: 5px 0px 0px 5px;
  display: flex;
  justify-content: space-between;
}

li.done {
  text-decoration: line-through;
  color: rgba(0, 0, 0, 0.2);
}

li:hover {
  cursor: pointer;
  color: chocolate;
}

input {
  width: 100%;
  border: none;
  border-bottom: solid 1px rgba(0, 0, 0, 0.5);
}

input:focus {
  outline: none;
}

button {
  width: 150px;
  cursor: pointer;
  background-color: transparent;
  border: none;
  color: cadetblue;
  border: 1px solid;
  border-radius: 5px;
  font-size: normal;
}

button:hover {
  color: chocolate;
}

button.delete {
  width: fit-content;
}
</style>
