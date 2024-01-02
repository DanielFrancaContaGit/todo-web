<script setup>
  const value = ref('')
  const selected = ref(true)
  let id = 0
  const todoList = ref([
    {id: id++, value: 'maçam'}, 
    {id: id++, value: 'banana'}
  ])
  const newTodo = ref('')
  function addTodo() {
    todoList.value.push({id: id++, value: value.value})
    value.value = ''
  }

  function removeTodo(todo) {
    todoList.value = todoList.value.filter((t) => t !== todo)
  }
</script>

<template>
  <div id="flexcon">
    <div id="container">
      <h2>Todo List</h2>
      <div  id="inputcase">
        <v-text-field v-model="value" label="escreva seu tudo aqui" variant="outlined" id='inp'/> 
        <v-btn @click="addTodo" id="btn">Adicionar ao todo</v-btn>
      </div>
      <ul>
        <li v-for="todo in todoList" :key="todo.id">
          <v-checkbox-btn />
          <p>{{ todo.value }}</p>
          <v-btn @click="removeTodo(todo)" icon="mdi-delete" size="x-small" />
        </li>
      </ul>
    </div>
  </div>
</template>