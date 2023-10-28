<script setup>
import { ref, onMounted, watch } from 'vue'

const headingInput = ref('');

// Watches for changes in the headingInput variable and stores the value in local storage
watch(headingInput, (newValue) => {
  localStorage.setItem('headingInput', newValue);
})

// Looks for value under the headingInput key and stores it in headingInput
onMounted(() => {
  headingInput.value = localStorage.getItem('headingInput') || ''
})
</script>

<template>
  <main>
    <section class="heading">
      <h1>What's up <input type="text" placeholder="name here" v-model="headingInput"></h1>
    </section>

    <section class="adding">
      <h3>What's on your todo list?</h3>
      <form action="submit">
        <input type="text">
        <button>Add todo</button>
      </form>
    </section>
  </main>
</template>