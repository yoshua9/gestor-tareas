<script setup lang="ts">
import type { Todo, TodoStatus } from "@/types";
import { reactive, ref } from "vue";
import useTodos from "@/store/useTodos";

interface Props {
  status: TodoStatus;
}
const props = defineProps<Props>();
const shouldDisplayForm = ref(false);

const { addNewTodo } = useTodos();

const newTodo = reactive<Omit<Todo, "id">>({
  title: "",
  description: "",
  status: props.status,
});

const resetForm = () => {
  shouldDisplayForm.value = false;
  newTodo.title = "";
  newTodo.description = "";
};

const handleOnSubmit = () => {
  addNewTodo({
    id: Math.random() * 10000000000000000,
    ...newTodo,
  });

  resetForm();
};
</script>

<template>
  <div class="add-container">
    <h3
      v-if="!shouldDisplayForm"
      @click="shouldDisplayForm = !shouldDisplayForm"
      class="add"
      :class="props.status"
    >
      +
    </h3>
    <template v-else>
      <div class="add-to-do__modal flex f-column justify-center align-center">
        <form @submit.prevent="handleOnSubmit">
          <div class="field">
            <input type="text" placeholder="Title" v-model="newTodo.title" />
          </div>

          <div class="field">
            <input
              type="text"
              placeholder="Description"
              v-model="newTodo.description"
            />
          </div>

          <button class="btn btn-create" type="submit">Create</button>
          <button class="btn btn-cancel" @click="resetForm">Cancel</button>
        </form>
      </div>
    </template>
  </div>
</template>

<style lang="scss" scoped>
$font-size: 16px;
.add-container {
  padding: 20px;

  .add-to-do__modal {
    position: fixed;
    background: #0000006b;
    width: 100vw;
    height: 100Vh;
    left: 0;
    top: 0;
    z-index: 100;

    form {
      padding: 50px;
      background: var(--vt-c-bg-card);
      width: 400px;
      border-radius: 8px;
    }
  }

  .add {
    font-size: 30px;
    font-weight: 500;
    cursor: pointer;
    font-family: fantasy;

    &.pending {
      color: var(--vt-c-status-pending);
    }

    &.inProgress {
      color: var(--vt-c-status-in-progress);
    }

    &.completed {
      color: var(--vt-c-status-completed);
    }
  }

  .field input {
    font-family: "nunito", sans-serif;
    font-size: $font-size;
    line-height: 28px;
    padding: 8px 16px;
    width: 100%;
    min-height: 44px;
    border: unset;
    border-radius: 4px;
    margin-bottom: 4px;
    outline-color: rgb(84 105 212 / 0.5);
    background-color: rgb(255, 255, 255);
    box-shadow: rgba(0, 0, 0, 0) 0px 0px 0px 0px, rgba(0, 0, 0, 0) 0px 0px 0px 0px, rgba(0, 0, 0, 0) 0px 0px 0px 0px, rgba(60, 66, 87, 0.16) 0px 0px 0px 1px, rgba(0, 0, 0, 0) 0px 0px 0px 0px, rgba(0, 0, 0, 0) 0px 0px 0px 0px, rgba(0, 0, 0, 0) 0px 0px 0px 0px;
    margin-bottom: 20px;
  }

  .btn {
    font-family: "nunito", sans-serif;
    color: #fff;
    cursor: pointer;
    border: 0;
    font-size: $font-size;
    padding: 8px 16px;
    width: 100%;
    border-radius: 4px;
    margin-bottom: 2px;
  }

  .btn-create {
    background-color: rgb(84, 105, 212);
  }

  .btn-cancel {
    background-color: rgb(227, 45, 32);
  }
}
</style>
