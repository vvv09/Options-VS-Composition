<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png">
        <h3>You have {{ todosCount }} Todos!</h3>
        <div>
            <input
                    v-model="newTodoName"
                    placeholder="Add a Todo"
                    type="text"
                    @keyup.enter="addTodo"
            >
        </div>
        <div>
            <ul>
                <li
                        v-for="(todo, index) in todos"
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

    import {ref, computed, watch} from 'vue';

    export default {

        setup() {
            let newTodoName = ref('') //ref - чтобы переменная была реактивной
            let todos = ref([])
            /*
             Т.к. переменная swearwords меняться не будет, то его не нужно делать
             реактивной и не тужно передавать в блоке return
             Просто сделать константой
            */
            const swearwords = ['fart', 'butt hair', 'willy']


            function addTodo() {
                let newTodo = {
                    id: Date.now(),
                    name: newTodoName.value
                }
                todos.value.push(newTodo)
                newTodoName.value = ''
            }

            function deleteTodo(index) {
                todos.value.splice(index, 1)
            }

            let todosCount = computed(() => {
                return todos.value.length
            })

            watch(newTodoName, (newValue) => {
                if (swearwords.includes(newValue.toLowerCase())) { // нет .value т.к. swearwords - не реактивная переменная
                    newTodoName.value = ''
                    alert('You must NEVER say ' + newValue + '!!')
                }
            })

            return {
                newTodoName,
                todos,
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

