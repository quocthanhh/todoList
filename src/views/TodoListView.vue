<template>
    <div class="container">
        <h1>Todo list </h1>
        <div class="main-content">
            <div class="left-content">
                <div class="upper-content">
                    <div class="filter-list">
                        <select v-model="selectedOptions">
                            <option selected> All</option>
                            <option v-for="(option, index) in select_list" :value="option.value" :key="index">{{
                                option.value }}
                            </option>
                        </select>
                    </div>
                    <div class="input-btn">
                        <div v-if="test === 1">
                            <form v-on:submit.prevent="addNewTodo" class="input-task">
                                <input v-model="newTodo" id="new-todo" placeholder="Input task" />
                                <button id="Add-btn">Add</button>
                            </form>
                        </div>
                        <div v-else class="input-task">
                            <input v-model="newTodo" id="new-todo" placeholder="Edit task" />
                            <button id="Add-btn" @click="updateClick">Update</button>
                        </div>
                    </div>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>Stt</th>
                            <th>Content</th>
                            <th>Status</th>
                            <th>#</th>
                            <th>#</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(todo, index) in filteredItems" :key="index">
                            <td>{{ index + 1 }}</td>
                            <td>{{ todo.content }}</td>
                            <td><input type="checkbox" v-model=todo.isCompleted />completed</td>
                            <td><button id="edit-btn" @click="editTodo(todo.content, index)">Edit</button></td>
                            <td><button id="remove-btn" @click="removeTodo(todo)">Remove</button></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="right-content">
                <h2>Description</h2>
                <div class="description-content">
                    <div>completed task: {{ countCompletedTasks }}</div>
                    <div>incompleted task: {{ countIncompletedTasks }} </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script lang="ts">
import { defineComponent } from 'vue'
import type Todo from '../types/todo';
import { ref } from 'vue';

export default defineComponent({
    name: "TodoList",
    components: {},
    setup() {
        const todos = ref<Todo[]>([
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
        ])
        const newTodo = ref("")
        const selectedOptions = ref("All")
        const select_list = ref([
            { value: "Completed todo" },
            { value: "Incompleted todo" }
        ])
        const isCompleted = ref(false)
        const newTodoId = ref(0)
        const test = ref(1)
        const test_index = ref(0)

        const removeTodo = (todo: Object) => {
            todos.value = todos.value.filter(item => item !== todo)
        }

        return { todos, newTodo, isCompleted, newTodoId, removeTodo, test, test_index, selectedOptions, select_list }
    },
    data() {
        return {

        }
    },
    mounted() {
    },
    methods: {
        addNewTodo() {
            this.todos.push({
                id: this.newTodoId++,
                content: this.newTodo,
                isCompleted: this.isCompleted,
            })
            this.newTodo = ''
        },
        deleteTask(index: number) {
            this.todos.splice(index, 1)
        },
        filterTodos(todos: Array<Todo>) {
            todos = this.todos.filter(todo => { todo.isCompleted === true })
            return todos
        },
        editTodo(newVal: string, index: number) {
            this.test = 2
            this.test_index = index
            this.newTodo = newVal
        },
        updateClick() {
            this.test = 1
            this.todos.splice(this.test_index, 1, {
                id: this.newTodoId++,
                content: this.newTodo,
                isCompleted: this.isCompleted,
            })
            this.newTodo = ''
        }
    },
    computed: {
        countCompletedTasks() {
            return this.todos.filter((todo) => {
                return todo.isCompleted === true
            }).length
        },
        countIncompletedTasks() {
            return this.todos.filter((todo) => {
                return todo.isCompleted === false
            }).length
        },
        filteredItems() {
            if (this.selectedOptions === "Completed todo") {
                return this.todos.filter(todo => todo.isCompleted === true)
            }
            else if (this.selectedOptions === "Incompleted todo") {
                return this.todos.filter(todo => todo.isCompleted === false)
            }
            else {
                return this.todos
            }
        }
    },
    watch: {
        countCompletedTasks(newVal, oldVal) {
            console.log("change number of completed todo from", oldVal, "to", newVal)
        },
        countIncompletedTasks(newVal, oldVal) {
            console.log("change number of incompleted todo from", oldVal, "to", newVal)
        }
    }
})
</script>
  
<style scoped>
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    flex-direction: column;
    /* top: 100px; */
    position: relative;
    left: 0;
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

.filter-list {
    width: 100%;
}

select {
    padding: 3px 0;
    cursor: pointer;
}

li {
    font-size: 15px;
    font-weight: 500;
}

table {
    width: 100%;
    margin-top: 20px;
}

th,
td {
    border: 1px solid black;
    text-align: center;
}

th {
    background-color: #2185d0;
    color: #fff;
}

td {
    padding: 5px 0px;
}

.left-content {
    display: flex;
    width: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
    top: 50px;
    left: 0px;
}

.input-task {
    display: flex;
}

.right-content {
    width: 100%;
    display: flex;
    align-items: center;
    flex-direction: column;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    margin: 0px 50px;
    height: 200px;
    position: relative;
    top: 50px;
    right: 0px;
}

.right-content h2 {
    margin: 10px 0px;
    font-weight: 500;
    color: #2185d0;
}

.main-content {
    display: flex;
    justify-content: center;
    width: 100%;
    flex-direction: row;
}

#edit-btn {
    opacity: 1;
    cursor: pointer;
}

#edit-btn:hover {
    opacity: 0.7;
}

.upper-content {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}

.description-content {
    display: flex;
    justify-content: space-evenly;
    width: 100%;
}

.description-content > div {
    font-size: 18px;
    font-weight: 500;
}

</style>
  