<script setup>
    const { data, pending, error, refresh } = await useFetch('http://127.0.0.1:8000/todo/', {
        lazy: true,
        headers: { authorization: "Basic YWRtaW46YWRtaW4=" },
    })

</script>

<template>
    <h1>todo fetch</h1>
    <h2>{{ data }}</h2>
    <h2>{{ error }}</h2>
    <li v-for="todo in data">
        <p>{{ todo.content }}</p>
    </li>
</template>