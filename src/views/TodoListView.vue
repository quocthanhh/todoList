<template>
    <div class="container">
        <h1>Todo list </h1>
        <div class="input-task">
            <form v-on:submit.prevent="addNewTodo">
                <input v-model="newTask" id="new-todo" placeholder="Input task" />
                <button id="Add-btn">Add</button>
            </form>
        </div>
        <TodoItem :todos="todos" @remove="deleteTask" :items="items" />
        <!-- <ol id="list-task">
            <li v-for="(task, index) in todos" :key="index">
                {{ task.content }}
                <button @click="deleteTask(index)">Remove</button>
            </li>
        </ol> -->
    </div>

    <!-- <ul>
      <todo-item
        v-for="(todo, index) in todos"
        :key="todo.id"
        :title="todo.title"
        @remove="todos.splice(index, 1)"
      ></todo-item>
    </ul>  -->
</template>
  
<script lang="ts">
import TodoItem from '../components/TodoItem.vue';

export interface IItem {
    id: number,
    content: string,
    isCompleted: boolean,
}

export interface ITodo {
    items: IItem[]
}

const todoList: ITodo = {
  items: [
    {
      id: 1,
      content: 'ABC',
      isCompleted: false,
    },

    {
      id: 2,
      content: 'ABC',
      isCompleted: true,
    },

    {
      id: 3,
      content: 'ABC',
      isCompleted: false,
    },
  ],
};

export default {
    name: "TodoList",
    components: { TodoItem },
    props: {
        // todos: Array<any>,
        // items: Array,
    },
    data() {
        return {
            newTask: '',
            isCompleted: false,
            todos: [
                {
                    id: 1,
                    content: 'Do the dishes',
                    isCompleted: true,
                },
                {
                    id: 2,
                    content: 'Take out the trash',
                    isCompleted: false,
                },
                {
                    id: 3,
                    content: 'Mow the lawn',
                    isCompleted: true,
                }
            ],
            items: todoList,
            nextTaskId: 4
        }
    },
    methods: {
        addNewTodo() {
            this.todos.push({
                id: this.nextTaskId++,
                content: this.newTask,
                isCompleted: this.isCompleted,
            })
            this.newTask = ''
        },
        deleteTask(index: number) {
            this.todos.splice(index, 1)
        }
    }
}
</script>
  
<style scoped>
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    flex-direction: column;
}

h1 {
    color: #2185d0;
    margin: 10px;
    font-family: 600;
    font-size: 40px;
}

input {
    padding: 10px 5px 7px 10px;
    margin-right: 10px;
    border-radius: 5px;
}

input::placeholder {
    padding-bottom: 2px;
}

#Add-btn {
    text-decoration: none;
    color: white;
    border: none;
    border-radius: 5px;
    padding: 5px 15px 6px 15px;
    background-color: #2185d0;
    font-size: 20px;
    cursor: pointer;
    opacity: 1;
}

#Add-btn:hover {
    opacity: 0.8;
}

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
  