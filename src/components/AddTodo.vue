<template>
    <div class="wrap">
        <div class="form-wrapper">
            <form class="form" @submit.prevent="onSubmit">
                <input
                    type="text"
                    placeholder="Add Todo"
                    class="input text-blue-grey-darken-3"
                    v-model="title"
                >
                <v-btn color="light-green" class="button" type="submit">
                    <v-icon size="x-large" color="#fff">mdi-plus</v-icon>
                </v-btn>
            </form>
            <p
                class="text-sm-body-2 text-red-lighten-2 pa-2"
                v-show="!isValid"
            >{{warningText}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddTodo',
    data() {
        return {
            title: '',
            isValid: true,
            warningText: ''
        }
    },
    methods: {
        onSubmit() {
            if (this.title.length < 3) {
                this.isValid = false;
                this.warningText = 'too short text';
            } else if (this.title.length > 64) {
                this.isValid = false;
                this.warningText = 'too long text';
            } else {
                this.isValid = true;
                this.warningText = '';

                const today = new Date();
                const newTodo = {
                    id: Date.now(),
                    title: this.title,
                    date: {
                        dd: today.getDate(),
                        mm: today.getMonth() + 1,
                        yyyy: today.getFullYear()
                    },
                    isReadOnly: true
                }
                this.$emit('add-todo', newTodo);
                this.title = '';
            }
        }
    }
}
</script>

<style scoped>
    .wrap {
        display: flex;
        justify-content: center;
    }

    .form-wrapper {
        width: 500px;
    }

    .form {
        display: flex;
    }

    .input {
        width: 100%;
        outline: none;
        padding: 5px;
        border-bottom: 1px solid #9e9e9e;
    }

    .button {
        border-radius: 0%;
        box-shadow: none;
        min-width: 0;
    }

    .v-btn {
        width: 36px;
    }
</style>