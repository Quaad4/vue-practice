<template>

    <h1>Users Manager</h1>

    <form @submit.prevent="addUser">
        <label for="newName">Enter a new name: </label>
        <input id="newName" type="text" v-model="newName">
        <button aria-label="Submit a new name" :disabled="newNameInputDisabled" type="submit">Submit</button>
    </form>

    <ul>
        <li v-for="user in users" :key="user.id">
            <div v-if="editedId !== user.id">
                {{ user.name }}
                <button @click="openEditor(user)">Edit</button>
            </div>
            <div v-else>
                <form @submit.prevent="saveEdit(user)">
                    <input ref="editInput" type="text" v-model="editedName">
                    <button aria-label="Save the new name" :disabled="editedNameInputDisabled" type="submit">Save</button>
                </form>
                <button aria-label="Cancel editing" @click="cancelEditing">Cancel</button>
            </div>
            <button @click="deleteUser(user)">Delete</button>
        </li>
    </ul>

</template>

<script>
    import { nextTick } from 'vue'

    export default {
        data() {
            return {
                users: [ 
                    {id: 1, name: 'Alex' },
                    {id: 2, name: 'Yasemin' }, 
                    {id: 3, name: 'Milo'}
                ],
                newName: "",
                editedId: "",
                editedName: "",
            }
        },
        methods: {
            addUser() {
                if(this.newNameInputDisabled) return
                const newId = this.users.length ? Math.max(...this.users.map(user => user.id)) + 1 : 1
                this.users.push(
                    { id: newId, name:  this.trimmedNewName}
                )

                this.newName = ''
            },
            openEditor(user) {
                this.editedId = user.id
                this.editedName = user.name

                nextTick(() => {
                    const input = Array.isArray(this.$refs.editInput) ? this.$refs.editInput[0] : this.$refs.editInput
                    input.focus()
                })
            },
            saveEdit(user) {
                if(!this.editedNameInputDisabled) {
                    user.name = this.trimmedEditedName
                }
                this.editedId = ""
                this.editedName = ""
            },
            cancelEditing() {
                this.editedId = ""
                this.editedName = ""
            },
            deleteUser(deletedUser) {
                if (confirm(`Delete ${deletedUser.name}?`)) {
                    this.users = this.users.filter(user => user.id !== deletedUser.id)
                    if (this.editedId === deletedUser.id) {
                    this.cancelEditing()
                    }
                }
            }
        },
        computed: {
            trimmedNewName() {
                return this.newName.trim()
            },
            newNameInputDisabled() {
                return this.trimmedNewName.length === 0
            },
            trimmedEditedName() {
                return this.editedName.trim()
            },
            editedNameInputDisabled() {
                return this.trimmedEditedName.length === 0
            },
        },
    }
</script>