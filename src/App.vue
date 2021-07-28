<template>
    <v-app class="app-main">
        <v-container>
            <v-row justify="center" class="mt-5 mb-5">
                <h1 class="text-grey">TODO list</h1>
            </v-row>

            <AddTodo
                @add-todo="addTodo"
            />

            <TodoList
                v-bind:todos="todos"
                @remove-todo="removeTodo"
                @edit-todo="editTodo"
                @save-edited-todo="saveEditedTodo"
            />

        </v-container>
    </v-app>
</template>

<script>
import TodoList from './components/TodoList.vue';
import AddTodo from './components/AddTodo.vue';

export default {
    name: 'App',

    components: {
        TodoList,
        AddTodo
    },

    data: () => ({
        todos: []
    }),

    mounted() {
        this.getTodos();
        this.storageEvent();
    },

    methods: {
        getTodos() {
            if (localStorage.getItem('todos')) {
                this.todos = JSON.parse(localStorage.getItem('todos'));
            }
        },
        addTodo(todo) {
            this.todos.push(todo);
        },
        removeTodo(id) {
            this.todos = this.todos.filter(todo => todo.id !== id);
        },
        editTodo(index) {
            this.todos[index].isReadOnly = false;
        },
        saveEditedTodo(index, value) {
            this.todos[index].isReadOnly = true;
            this.todos[index].title = value;
        },
        storageEvent() {
            window.onstorage = event => {
                if (event.key != 'todos') return;
                this.getTodos();
            };
        }
    },

    watch: {
        todos: {
            handler: function(updatedList) {
                localStorage.setItem('todos', JSON.stringify(updatedList));
            },
            deep: true
        }
    }
}
</script>

<style scoped>
    .app-main {
        background: #e1e1e1;
    }
</style>