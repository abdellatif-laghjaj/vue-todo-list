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
    {{ todos_asc }}
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
  if(input_content.value.trim() === '' || input_category === null) {
    return
  }

  //create todo object
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),     
  })
}

//sort todos by date
const todos_asc = computed(() => { 
  return todos.value.sort((a, b) => b.createdAt - a.createdAt)
})


//save data to local storage
watch(todos, newVal =>{
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