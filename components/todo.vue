<script setup>
  const value = ref('')
  const todoData = ref(null)

  const serverUrl = 'http://127.0.0.1:8000/todo/'

  async function fetchData() {
    todoData.value = null

    const { data } = await useFetch(serverUrl, {
      lazy: true,
      headers: { authorization: "Basic YWRtaW46YWRtaW4=" },
    })

    todoData.value = await data.value
  }

  async function submit() {
    const response = await useFetch(serverUrl, {
        headers: { authorization: "Basic YWRtaW46YWRtaW4=" },
        method: 'POST',
        body: { "content": value.value }
    })
    value.value = ''
    fetchData()
    return response
  }

  async function deleteTodo(todoId) {
    const response = await useFetch(serverUrl, {
      headers: { authorization: "Basic YWRtaW46YWRtaW4=" },
      method: 'DELETE',
      body: { 'id': todoId }
    })

    fetchData()
    return response
  }

  fetchData()
</script>

<template>
  <div id="flexcon">
    <div id="container">
      <h2>Todo List</h2>
      <div  id="inputcase">
        <v-text-field v-model="value" label="escreva seu tudo aqui" variant="outlined" id='inp'/> 
        <v-btn @click="submit" id="btn">Adicionar ao todo</v-btn>
      </div>
      <ul>
        <p v-if="!todoData" id="load">Loading...</p>
        <li v-for="todo in todoData" :key="todo.id">
          <v-checkbox-btn />
          <p>{{ todo.content }}</p>
          <v-btn @click="deleteTodo(todo.id)" icon="mdi-delete" size="x-small" />
        </li>
      </ul>
    </div>
  </div>
</template>