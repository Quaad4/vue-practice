<template>
    <h1 id="main-heading">My ToDos!</h1>

    <main>

        <section>
            <ul id="todo-list">
                <div id="filter-buttons">
                    <button v-for="filterButton in filterButtons"
                        :class="{ 'active-filter': filterBy === filterButton.filter }"
                        @click="setFilterBy(filterButton.filter)"
                    >
                        {{ filterButton.filter }}
                    </button>
                </div>
                <li v-for="todo in filteredData" :key="todo.id">
                    <div v-if="editTodoId !== todo.id">
                        <p class="action" :class="{ done: todo.done }">{{ todo.action }}</p>
                        <p>Created at: {{ todo.createdAt.toDateString() }}</p>
                        <button @click="openTodoEditor(todo)" class="edit-button">EDIT</button>
                        <button @click="deleteTodo(todo)" class="delete-button">DELETE</button>
                        <div class="done-checkbox">
                            <label :for="`done-checkbox-item-${todo.id}`">Done: </label>
                            <input :id="`done-checkbox-item-${todo.id}`" type="checkbox" v-model="todo.done">
                        </div>
                    </div>
                    <div v-else>
                        <input class="edit-input" type="text" v-model="editTodoText">
                        <button :disabled="!editTodoIsValid" @click="saveEdit(todo)" class="save-button">SAVE</button>
                        <button @click="deleteTodo(todo)" class="delete-button">DELETE</button>
                    </div>
                </li>
            </ul>
        </section>
    
        <section>
            <form id="form-input" @submit.prevent="createNewTodo">
                <label for="action-input">Add a new Todo: </label>
                <input id="action-input" type="text" placeholder="Enter your Action here" v-model="newTodo">
                <button aria-label="Submit a new Todo" :disabled="!newTodoIsValid" type="submit">SUBMIT</button>
            </form>
        </section>

    </main>

</template>

<script>

    export default {
        data() {
            return {
                todos: [{id: 1, action: 'Eat breakfast', done: true, createdAt: new Date()}, {id: 2, action: 'walk the dog', done: false, createdAt: new Date()}],
                filterButtons: [{id: 1, filter: 'all'}, {id: 2, filter: 'active'}, {id: 3, filter: 'completed'}],
                newTodo: '',
                editTodoId: '',
                editTodoText: '',
                filterBy: 'all',
            }
        },
        methods: {
            createNewTodo() {
                if(this.newTodoIsValid) {
                    const newId = this.todos.length ? Math.max(...this.todos.map(todo => todo.id)) + 1 : 1;
                    this.todos.push(
                        {
                            id: newId,
                            action: this.newTodo,
                            done: false,
                            createdAt: new Date()
                        }
                    )
                }
                this.newTodo = ''
            },
            openTodoEditor(todo) {
                this.editTodoId = todo.id
                this.editTodoText = todo.action
            },
            saveEdit(todo) {
                if(this.editTodoIsValid) {
                    todo.action = this.editTodoText
                }
                this.editTodoId = ''
                this.editTodoText = ''
            }, 
            deleteTodo(deletedTodo) {
                this.todos = this.todos.filter(todo => todo.id !== deletedTodo.id)
            },
            setFilterBy(filterByChosen) {
                this.filterBy = filterByChosen
            }
        },
        computed: {
            newTodoTrimmed() {
                return this.newTodo.trim()
            },
            newTodoIsValid() {
                return this.newTodoTrimmed.length
            },
            editTodoTrimmed() {
                return this.editTodoText.trim()  
            },
            editTodoIsValid() {
                return this.editTodoTrimmed.length
            },
            filteredData() {
                if(this.filterBy === 'active') {
                    return this.todos.filter(todo => !todo.done)
                } else if(this.filterBy === 'completed') {
                    return this.todos.filter(todo => todo.done)
                }
                return this.todos
            }
        },
    }    
</script>

<style>
    @import './newStyle.css';
</style>