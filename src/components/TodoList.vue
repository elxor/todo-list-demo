<template>
    <div class="wrap">
        <div class="todo-list mt-5">
            <TodoItem
                v-for="(todo, i) of todos"
                v-bind:key="todo.id"
                v-bind:todo="todo"
                v-bind:index="i"
                v-on:remove-todo="removeTodo"
                v-on:edit-todo="editTodo"
                v-on:save-edited-todo="saveEditedTodo"
            />
        </div>
    </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
    name: 'TodoList',
    props: ['todos'],
    components: {
        TodoItem
    },
    methods: {
        removeTodo(id) {
            this.$emit('remove-todo', id);
        },
        editTodo(index) {
            this.$emit('edit-todo', index);
        },
        saveEditedTodo(index, value) {
            this.$emit('save-edited-todo', index, value);
        }
    }
}
</script>

<style scoped>
    .wrap {
        display: flex;
        justify-content: center;
    }

    .todo-list {
        width: 500px;
        background: #fff;
    }
</style>
