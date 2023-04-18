<template>
    <ol id="list-task">
        <li v-for="(todo, index) in todos" :key="index">
            {{ todo.content }}
            <button @click="$emit('remove')">Remove</button>
        </li>
    </ol>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';
import type Todo from '@/types/todo';

export default defineComponent( {
    name: "TodoItem",
    props: {
        todos : {
            required:true,
            type: Array as PropType<Todo[]>
        }
    },
    emits: ['remove'],
})
</script>

<style>
#list-task {
    margin-top: 30px;
    display: flex;
    justify-content: left;
    flex-direction: column;
}

li {
    font-size: 15px;
    font-weight: 500;
}
</style>