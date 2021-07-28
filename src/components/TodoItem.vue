<template>
    <div class="wrapper">
        <div class="todo-item">
            <span class="numid text-blue-grey darken-4">
                <strong>{{index + 1}}.</strong>
            </span>
            <span
                class="text-blue"
                v-show="todo.isReadOnly"
            >
                {{todo.title}}
            </span>

            <input
                class="text-blue input-item"
                v-bind:readonly="todo.isReadOnly"
                v-show="!todo.isReadOnly"
                v-model="itemValue"
            >
                
            <v-icon
                color="grey"
                class="edit-icon"
                v-show="todo.isReadOnly"
                v-on:click="$emit('edit-todo', index)"
            >mdi-pencil-outline</v-icon>

            <v-icon
                color="grey"
                class="save-icon"
                v-show="!todo.isReadOnly"
                v-on:click="saveEditedHandler"
            >mdi-content-save</v-icon>

            <v-icon
                color="grey"
                class="icon-remove"
                v-on:click="$emit('remove-todo', todo.id)"
            >mdi-close-circle-outline</v-icon>
        </div>
        <div class="date-wrapper mt-3">
            <span
                v-show="!isValid"
                class="text-sm-body-2 text-red-lighten-2"
            >
                {{warningText}}
            </span>
            <span class="text-sm-body-2 text-grey date">
                date: {{todo.date.mm}}/{{todo.date.dd}}/{{todo.date.yyyy}}
            </span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoItem',
    props: {
        todo: {
            type: Object,
            required: true
        },
        index: Number
    },
    data() {
        return {
            itemValue: '',
            isValid: false,
            warningText: ''
        }
    },
    mounted() {
        this.getTodosItemValue();
    },
    methods: {
        getTodosItemValue() {
            this.itemValue = this.todo.title;
        },
        saveEditedHandler() {
            if (this.itemValue.length < 3) {
                this.isValid = false;
                this.warningText = 'too short text';
            } else if (this.itemValue.length > 64) {
                this.isValid = false;
                this.warningText = 'too long text';
            } else {
                this.isValid = true;
                this.warningText = '';
                this.$emit('save-edited-todo', this.index, this.itemValue);
            }
        }
    }
}
</script>

<style scoped>
    .wrapper {
        padding: 20px 15px;
        border-bottom: 1px solid #e1e1e1;
    }
    .todo-item {
        display: grid;
        grid-template-columns: 20px 1fr 24px 24px;
        grid-column-gap: 10px;
    }

    .edit-icon,
    .save-icon {
        cursor: pointer;
    }

    .date-wrapper {
        display: flex;
    }

    .date {
        margin-left: auto;
    }

    .icon-remove {
        cursor: pointer;
    }

    .input-item {
        outline: none;
        border: 1px solid #9e9e9e;
        min-width: 50px;
    }
</style>