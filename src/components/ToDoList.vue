<script setup>
import { ref, computed } from 'vue'

const todos = ref([

])

const text = ref('')

function Submit() {
  if (text.value.trim()) {
    
    todos.value.push({
      id: Date.now(), 
      text: text.value,
      done: false
    })
    
    text.value = ''
  }
}

function Remove(id) {
    todos.value = todos.value.filter(todo => todo.id !== id)
}
function Update(id) {
    const targetTodo = todos.value.find(todo => todo.id === id)
  
  if (targetTodo) {
    const newText = prompt('แก้ไขข้อความ Todo:', targetTodo.text)
    
    if (newText !== null && newText.trim() !== '') {
      targetTodo.text = newText.trim()
    }
  }
}

function Tick(id) {
    todos.value = todos.value.map 
    (todo => todo.id === id ? { ...todo, done: !todo.done } : todo
)}


const remaining1 = computed(() =>
  todos.value.filter(todo => todo.done).length
)
const remaining2 = computed(() =>
  todos.value.filter(todo => !todo.done).length
)

</script>

<template>
  <h2>TodoList</h2>
  
  <form @submit.prevent="Submit">
    <label>Text : </label>
    <input id="input-text" type="text" v-model="text" />
    <button id="submit" type="submit"> Add </button>
    
  </form>

  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id">
        <input type="checkbox" v-on:click="Tick(todo.id)" />
      {{ index + 1 }}. {{ todo.text }}


        <button v-on:click="Update(todo.id)"> Update </button>
        <button v-on:click="Remove(todo.id)"> Remove </button>
    </li>
  </ul>

  <div>
    <strong>จำนวนที่เสร็จ {{ remaining1 }}</strong> <br>
    <strong>จำนวนที่ไม่เสร็จ {{ remaining2 }}</strong>
  </div>
    

</template>