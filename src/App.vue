<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome, <input type="text" placeholder="Your name" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What are you planning to do?</h4>
        <input type="text" placeholder="e.g Buy milk" v-model="input_content" />
        <h4>Pick your todo category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" value="personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add Todo">
      </form>

    </section>

    <section class="todo-list">
      <div v-if="todos.length > 0">
        <h3>Your todos</h3>
        <div class="list">
          <div v-for="(todo, index) in todos_asc" :key="index" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`"></span>
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="deleteTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </div>

      <div v-else>
        <center>
          <h3>You have no todos</h3>
        </center>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

//add todo
const addTodo = () => {
  if (input_content.value.trim() === '' || input_category === null) {
    return
  }

  //create todo object
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  })

  //clear input
  input_content.value = ''
  input_category.value = null
}

//delete todo
const deleteTodo = (todo) => {
  return todos.value = todos.value.filter(t => t !== todo)
}

//sort todos by date
const todos_asc = computed(() => {
  return todos.value.sort((a, b) => b.createdAt - a.createdAt)
})


//save data to local storage
watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, newVal => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>