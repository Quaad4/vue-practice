<script setup>
  import Todo from './components/todo.vue';
  import UserManager from './components/UserManager.vue';
  import UserManager2 from './components/UserManager2.vue';
</script>

<template>
  <!-- <Todo
    @count-passed-ten="handleCountPassedTen"
  ></Todo> -->
  <!-- <UserManager></UserManager> -->
   <UserManager2></UserManager2>
</template>

<script>
  export default {
    methods: {
      handleCountPassedTen(value) {
        console.log(`COUNT HAS PASSED ${value}`)
      }
    }
  }
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
