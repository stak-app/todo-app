<template>
  <section class="todoapp">
    <header class="header">
      <h1>simple todos</h1>
      <form @submit.prevent="add">
        <input v-model="newTodo" class="new-todo" placeholder="What needs to be done?" autofocus>
        <button class="hidden" type="submit">Add</button>
      </form>
    </header>
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li :class="todo.checked ? 'completed' : ''" v-for="todo in filteredTodos" :key="todo">
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.checked">
            <label>{{todo.title}}</label>
            <button class="destroy" @click="remove(todo)"></button>
          </div>
        </li>        
      </ul>
    </section>
    <footer class="footer">
      <span class="todo-count">{{todos.length}} <span v-if="todos.length === 0 || todos.length > 1">todos</span><span v-else>todo</span></span>
      <ul class="filters">
        <li>
          <a @click="mode = 'all'" :class="mode === 'all' ? 'selected' : ''">All</a>
        </li>
        <li>
          <a @click="mode = 'active'" :class="mode === 'active' ? 'selected' : ''">Active</a>
        </li>
        <li>
          <a @click="mode = 'completed'" :class="mode === 'completed' ? 'selected' : ''">Completed</a>
        </li>
      </ul>
      <button class="clear-completed" @click="clearCompleted">Clear completed</button>
    </footer>
  </section>
  <footer class="info">
    <p>Double-click to edit a todo</p>
    <p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
  </footer>
</template>

<script setup>
import { ref, computed } from "@vue/reactivity";

const todos = ref([]);
const newTodo = ref('');
const mode = ref('all');

const add = function() {
  todos.value.push({title: newTodo.value, checked: false})
  newTodo.value = '';
}

const remove = function(item) {
  todos.value.splice(todos.value.indexOf(item))
}

const clearCompleted = function() {
  todos.value = todos.value.filter(todo => !todo.checked)
}

const filteredTodos = computed(() => todos.value.filter(todo => {
  return mode.value === 'all' || (mode.value === 'completed' && todo.checked) || (mode.value === 'active' && !todo.checked)
}))
</script>