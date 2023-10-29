<script setup>
import { ref, onMounted, watch } from 'vue'

let id = 0;
const headingInput = ref('');
const newTodo = ref('');
const todos = ref([]);

// Watches for changes in the headingInput variable and stores the value in local storage
watch(headingInput, (newValue) => {
  localStorage.setItem('headingInput', newValue);
})

// Watches for changes in the todos variable and stores the value in local storage as a string
// deep: true, watches for changes in nested objects within the array 
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos));
}, {
  deep: true
});

// Looks for value under the headingInput key and stores it in headingInput variable
// Parse() - converts the JSON object in text format to a javascript object 
onMounted(() => {
  headingInput.value = localStorage.getItem('headingInput') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

// Pushing an object with id and text to todos variable + setting the input value back to an empty string
// If there is no value or only spaces the function will stop
function addTodo() {
  if (newTodo.value.trim() === '') {
    return
  }
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

// The function iterates over all the todo items and creates another array with the elements that pass the argument in the arrow function
function removeTodo(todo) {
  todos.value = todos.value.filter((item) => item !== todo)
}

function taskDone(todo) {
  todo.isDone = !todo.isDone;
}
</script>

<template>
  <main class="main">
    <section class="heading__section">
      <h1 class="heading">What's up <input class="heading__input" type="text" placeholder="name here"
          v-model="headingInput" /></h1>
    </section>

    <section class="adding__section">
      <h3>What's on your todo list?</h3>
      <form @submit.prevent="addTodo" class="adding__form">
        <input class="adding__input" type="text" v-model="newTodo" placeholder="write a todo" />
        <button class="adding__button">Add todo</button>
      </form>
    </section>

    <section class="list__section">
      <h3>Todo list</h3>
      <ul class="list__items">
        <li v-for="todo in todos" :id="todo.id" class="list__item">
          <div class="list__wrapper">
            <input id="check" type="checkbox" v-model="todo.isDone">
            <label for="check" :class="{ checked: todo.isDone }">{{ todo.text }}</label>
          </div>
          <button class="list__item-button" @click="removeTodo(todo)">Delete</button>
        </li>
      </ul>
    </section>
  </main>
</template>