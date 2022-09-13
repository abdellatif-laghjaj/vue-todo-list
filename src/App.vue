<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome, <input type="text" placeholder="Your name" v-model="name" />
      </h2>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

//sort todos by date
const todos_asc = computed(() => {
  todos.value.sort((a, b) => a.createdAt - b.createdAt)
})

watch(name, newVal => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})
</script>