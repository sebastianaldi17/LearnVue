<script>
let id = 0
export default {

    // Similkar to methods but is cached for performance
    computed: {
        filteredTodos() {
            if (this.hideCompleted) return this.todos.filter((todo) => { return !todo.done })
            return this.todos
        },
    },

    // Lifecycle on created
    created() {
        this.$emit('emitEvent', 'Hello from LearnComponent.vue')
    },

    // Data that can be displayed using {{}}
    data() {
        return {
            count: 0,
            hideCompleted: false,
            message: 'Sample text',
            newtodo: '',
            titleClass: 'title',
            textToSync: '',
            spammy: false,
            todos: [
                { done: false, id: id++, text: 'Learn HTML' }
            ]
        }
    },

    // Emits declare emittable things to the parent
    emits: ['emitEvent'],

    // Methods that can be called by html
    methods: {
        addTodo() {
            if (this.newtodo.trim().length <= 0) return
            this.todos.push({ done: false, id: id++, text: this.newtodo })
            this.newtodo = ''
        },
        increaseCount(x) {
            this.count += x
            if (this.count > 5) {
                this.spammy = true
            }
        },
        removeTodo(idtoremove) {
            this.todos = this.todos.filter((_, i) => {
                return this.todos[i].id !== idtoremove
            })
        },
        syncText() {
            this.message = this.textToSync
        },
        toggleHide() {
            this.hideCompleted = !this.hideCompleted
        },
    },

    // Runs when component is mounted
    // Similar to useEffect on react.js
    mounted() {
        setInterval(() => {
            this.$refs.spamtext.style.color = "#" + Math.floor(Math.random() * 16777215).toString(16);
        }, 250)
    },

    // Props is args supplied to component
    props: {
        passedVal: String
    },

    // Watches if variable in data(){} changed
    watch: {
        count(newClickCount) {
            console.log(`New click count: ${newClickCount}`)
            if (newClickCount > 5) {
                // this.spammy can be set here too
                alert("Oi, stop spammin")
            }
        }
    }
}
</script>

<template>
    <h1 :class="titleClass">{{ message }}</h1>
    <h1 v-if="spammy" ref="spamtext">dude don't spam</h1>
    <h1 v-else-if="count == 0">this is a v-else-if block that checks if count is 0</h1>
    <h1 v-else>this is a v-else block</h1>
    <button @click="increaseCount(1)">Press count: {{ count }}</button> <br />
    <input v-model="textToSync" @input="syncText" placeholder="Text above will be this">
    <h1>Todo List in vue</h1>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
            <button @click="removeTodo(todo.id)">X</button>
            <span :class="{ completed: todo.done }">{{ todo.text }}</span>
            <input type="checkbox" v-model="todo.done">
        </li>
    </ul>
    <form @submit.prevent="addTodo">
        <input v-model="newtodo">
        <button>Add todo</button>
    </form>
    <button @click="toggleHide">{{ hideCompleted ? "Show all" : "Hide completed" }}</button>
    <h1 v-if="passedVal && passedVal.trim().length > 0">{{ passedVal.trim() }}</h1>
    <h1 v-else>No props / empty string passed</h1>
    <h2>
        <slot>Fallback content if no message between LearnComponent</slot>
    </h2>
</template>

<style>
.completed {
    text-decoration: line-through;
}

.title {
    color: rgb(83, 107, 214);
}
</style>