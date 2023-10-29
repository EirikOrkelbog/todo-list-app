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
</script>

<template>
  <main>
    <section class="heading">
      <h1>What's up <input type="text" placeholder="name here" v-model="headingInput" /></h1>
    </section>

    <section class="adding">
      <h3>What's on your todo list?</h3>
      <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" />
        <button>Add todo</button>
      </form>
    </section>

    <section class="list">
      <h3>Todo list</h3>
      <ul>
        <li v-for="todo in todos" :id="todo.id">
          <input type="checkbox">
          <label>{{ todo.text }}</label>
          <button>X</button>
        </li>
      </ul>
    </section>
  </main>
</template>