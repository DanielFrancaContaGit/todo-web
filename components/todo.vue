<script setup>
  const value = ref('')
  const todoData = ref(null)

  const config = useRuntimeConfig()

  const serverUrl = config.public.SERVER_URL
  const loginToken = config.public.LOGIN_TOKEN

  
  async function fetchData() {
    // todoData.value = null
    const { data } = await useFetch(serverUrl, {
      lazy: true,
      headers: { authorization: "Basic " + loginToken },
    })

    todoData.value = await data.value 
  }

  // useAsyncData(
  //   'mountains',
  //   async () => await fetchData()
  // )

  async function submit() {
    await useFetch(serverUrl, {
        headers: { authorization: "Basic "+ loginToken },
        method: 'POST',
        body: { "content": value.value }
    })
    value.value = ''
    // await refreshNuxtData('mountains')
    fetchData()
  }

  async function deleteTodo(todoId) {
    await useFetch(serverUrl, {
      headers: { authorization: "Basic " + loginToken },
      method: 'DELETE',
      body: { 'id': todoId }
    })

    await refreshNuxtData('mountains')
    fetchData()
  }

  onNuxtReady(async () =>{
      fetchData()
  })
  // fetchData()
</script>

<template>
  <div id="flexcon">
    <div id="container">
      <h2>Todo List</h2> 
      <div id="inputcase">
        <v-text-field v-model="value" label="escreva seu tudo aqui" variant="outlined" id='inp'/> 
        <v-btn @click="submit" id="btn">Adicionar ao todo</v-btn>
      </div>
      <ul>
        <p v-show="!todoData" id="load">Loading...</p>
        <!-- <p id="load">{{ todoData }}</p> -->
        <li v-for="{ content, id } in todoData" :key="id">
          <v-checkbox-btn />
          <p>{{ content }}</p>
          <v-btn @click="deleteTodo(id)" icon="mdi-delete" size="x-small" />
        </li>
      </ul>
    </div>
  </div>
</template>