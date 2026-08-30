<script setup>
import { ref, computed } from 'vue'

const todos = ref([])
const text = ref('')
const url = ref('') 

function Submit() {
  if (text.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: text.value,
      img: url.value, 
      numlike: 0,
      done: false
    })
    text.value = ''
    url.value = '' 
  }
}

function Remove(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

function Update(id) {
  const targetTodo = todos.value.find(todo => todo.id === id)
  
  if (targetTodo) {
    const newText = prompt('แก้ไขข้อความโพส :', targetTodo.text)
    if (newText !== null && newText.trim() !== '') {
      targetTodo.text = newText.trim()
    }

    
    const newImg = prompt('แก้ไข URL รูปภาพ :', targetTodo.img)
    if (newImg !== null) { 
      targetTodo.img = newImg.trim()
    }
  }
}

function TickLike(id) {
  todos.value = todos.value.map(todo => {
    if (todo.id === id) {
      const newLiked = !todo.liked
      return { 
        ...todo, 
        liked: newLiked, 
        numlike: newLiked ? todo.numlike + 1 : todo.numlike - 1 
      }
    }
    return todo
  })
}

const totalLikes = computed(() => {
  return todos.value.reduce((sum, todo) => sum + todo.numlike, 0)
})
</script>

<template>
  <h2>โพสใหม่</h2>
  <form @submit.prevent="Submit">
    <label>เพิ่มโพส : </label>
    <input id="input-text" type="text" v-model="text" />
    <br>
    <label> url : </label>
    <!-- FIXED: Added v-model="url" here -->
    <input type="url" id="img-url" name="img-url" v-model="url">
    <button id="submit" type="submit"> Add </button>
  </form>

  <ul>
    <li v-for="(todo) in todos" :key="todo.id">
      <button :checked="todo.liked" @click="TickLike(todo.id)"> Like </button>
      {{ todo.text }}
      <button @click="Update(todo.id)"> Edit </button>
      <button @click="Remove(todo.id)"> Remove </button>
      <div>
        <img v-if="todo.img" :src="todo.img" alt="Todo image" style="max-width: 200px;"/>
        <strong>Like: {{ todo.numlike }}</strong>
        <br>
      </div>
    </li>
  </ul>
</template>



<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
