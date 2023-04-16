<script lang="ts">
import IItem from '../views/TodoListView.vue'
import ITodo from '../views/TodoListView.vue'


export default {
    name: "TodoItem",
    props: {
        todos : Array,
        items: Array<ITodo>,
    },
    emits: ['remove'],
    computed: {
        viewTodos: () => {
            console.log()
        }
    }
}
</script>

<template>
    <ol>
        <li v-for="(todo, index) in items" :key="index">
            {{ todo.content }}
            <button @click="$emit('remove')">Remove</button>
        </li>
    </ol>
</template>