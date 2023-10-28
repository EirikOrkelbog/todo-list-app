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

// Looks for value under the headingInput key and stores it in headingInput
onMounted(() => {
  headingInput.value = localStorage.getItem('headingInput') || ''
})

// Pushing an object with id and text to todos array + setting the input value back to an empty string 
function addTodo() {
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
        <input v-model="newTodo" />
        <button>Add todo</button>
      </form>
    </section>

    <section class="list">
      <h3>Todo list</h3>
      <ul>
        <li v-for="todo in todos" :id="todo.id">{{ todo.text }}</li>
      </ul>
    </section>
  </main>
</template>