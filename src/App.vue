<script setup lang="ts">
import TodoList from "./components/TodoList.vue";
</script>

<template>
  <div class="card shadow-1 flex f-column align-center">
    <h1>ToDo App</h1>

    <div class="card-body">
      <TodoList />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.card {
  min-height: 100vh;
  padding: 20px;
  border-radius: 15px;
  background-color: var(--vt-c-white-mute);

  h1 {
    color: #1a1a1a;
    font-size: 34px;
    margin-bottom: 20px;
  }

  .card-body {
    height: 100%;
    width: 100%;
  }
}
</style>
