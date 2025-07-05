<template>
  <div class="p-4">
    <h2 class="text-xl font-bold mb-4">User Manager</h2>

    <!-- ADD USER FORM -->
    <form @submit="addUser">
      <input
        v-model="newUserName"
        placeholder="Enter user name"
        class="border p-2 mr-2"
      />
      <button
        type="submit"
        class="bg-blue-500 text-white px-4 py-2 rounded"
        :disabled="newUserName.trim().length === 0"
      >
        Add User
      </button>
    </form>

    <!-- USER LIST -->
    <ul class="mt-4">
        <li
            v-for="user in users"
            :key="user.id"
            class="flex justify-between items-center border-b py-2"
        >
            <span v-if="editingUserId !== user.id">{{ user.name }}</span>
            <input
                v-else
                v-model="editedName"
                class="border p-1"
            />

            <div>
                <button
                    v-if="editingUserId !== user.id"
                    @click="startEditing(user)"
                    class="text-blue-500 mr-2"
                >
                    Edit
                </button>
                <button
                    v-else
                    @click="saveEdit(user)"
                    class="text-green-600 mr-2"
                >
                    Save
                </button>
                <button
                    @click="deleteUser(user.id)"
                    class="text-red-600"
                >
                    Delete
                </button>
            </div>
        </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
        users: [
        { id: 1, name: "Alice" },
        { id: 2, name: "Bob" },
        ],
        newUserName: "",
        editingUserId: null,
        editedName: "",
    };
  },
  methods: {
    addUser(e) {
      e.preventDefault();
      if (this.newUserName.trim() === "") return;

      const newId = this.users.length
        ? Math.max(...this.users.map((u) => u.id)) + 1
        : 1;

      this.users.push({
        id: newId,
        name: this.newUserName.trim(),
      });

      this.newUserName = "";
    },
    startEditing(user) {
        this.editingUserId = user.id;
        this.editedName = user.name;
    },
    saveEdit(user) {
    if (this.editedName.trim() !== "") {
        user.name = this.editedName.trim();
    }
        this.editingUserId = null;
        this.editedName = "";
    },
    deleteUser(id) {
        this.users = this.users.filter((user) => user.id !== id);
    }
  },
};
</script>

<style scoped>
/* optional styling */
</style>