<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png">
        <h3>You have {{ todosCount }} Todos!</h3>
        <div>
            <input
                    v-model="data.newTodoName"
                    placeholder="Add a Todo"
                    type="text"
                    @keyup.enter="addTodo"
            >
        </div>
        <div>
            <ul>
                <li
                        v-for="(todo, index) in data.todos"
                        :key="todo.id"
                >
                    <span>{{ todo.name }}</span>
                    <button @click="deleteTodo(index)">X</button>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>

    import { reactive,  computed, watch } from 'vue';

    export default {

        setup() {

            const swearwords = ['fart', 'butt hair', 'willy']

            let data = reactive({
                newTodoName: '',
                todos: [],
            })


            function addTodo() {
                let newTodo = {
                    id: Date.now(),
                    name: data.newTodoName
                }
                data.todos.push(newTodo)
                data.newTodoName = ''
            }

            function deleteTodo(index) {
                data.todos.splice(index, 1)
            }

            let todosCount = computed(() => {
                return data.todos.length
            })

            watch(data, (newValue) => {
                if (swearwords.includes(newValue.newTodoName.toLowerCase())) { // нет .value т.к. swearwords - не реактивная переменная
                    alert('You must NEVER say ' + newValue.newTodoName + '!!')
                    data.newTodoName = ''
                }
            })

            return {
                data,
                addTodo,
                deleteTodo,
                todosCount
            }
        }
    }
</script>

<style>
    ul {
        list-style: none;
        padding: 0;
        width: 200px;
        margin: 20px auto 0;
    }

    li {
        border: 1px solid;
        display: flex;
        margin-bottom: 10px;
    }

    li span {
        flex-grow: 1;
    }
</style>

