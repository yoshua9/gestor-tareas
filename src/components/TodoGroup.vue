<script setup lang="ts">
import { TodoStatus } from "@/types";
import Draggable from "vuedraggable";
import useTodos from "@/store/useTodos";
import CreateTodo from "./CreateTodo.vue";
import StatusHeader from "@/components/StatusHeader.vue";

interface STATUS {
  status: TodoStatus;
}

const status = defineProps<STATUS>();

const { getTodosByStatus, deleteTodo, updateTodo } = useTodos();
const todoList = getTodosByStatus(status.status);

const groupLabel = {
  [TodoStatus.Pending]: "To Do",
  [TodoStatus.InProgress]: "In Progress",
  [TodoStatus.Completed]: "Completed",
};

const onDraggableChange = (payload: any) => {
  if (payload?.added?.element?.status) {
    updateTodo(payload?.added?.element, status.status);
  }
};
</script>

<template>
  <div>
    <status-header :status="status.status">
      <CreateTodo :status="status.status" />
    </status-header>

    <div class="group-wrapper flex f-column justify-between">
      <div>
        <Draggable
          class="draggable"
          :list="todoList"
          group="todos"
          itemKey="id"
          @change="onDraggableChange"
        >
          <template #item="{ element: todo }">
            <li class="flex justify-between align-center">
              <div>
                <h2 class="todo-title">
                  {{ todo.title }}
                </h2>

                <div>
                  <span class="todo-description">{{ todo.description }}</span>
                </div>
              </div>

              <span class="delete-icon" @click="deleteTodo(todo)">x</span>
            </li>
          </template>
        </Draggable>
      </div>

    </div>
  </div>
</template>

<style lang="scss" scoped>
.group-wrapper {
  flex: 1;
  background-color: var(--vt-c-white);
  width: 300px;

  h3 {
    color: #1a1a1a;
    font-size: 22px;
    padding: 20px;
  }

  li {
    gap: 10px;
    border-radius: 8px;
    list-style-type: none;
    background-color: var(--vt-c-white-soft);
    color: rgb(0, 0, 0);
    padding: 10px;
    cursor: grab;
    margin-bottom: 10px;

    .todo-title {
      font-size: 20px;
      margin-bottom: 10px;
    }
  }

  .draggable {
    overflow-y: auto;
    height: 75vh;
    padding: 10px 20px;
  }

  .delete-icon {
    float: right;
    cursor: pointer;
    margin-top: 5px;
    font-weight: bold;
    font-size: 20px;
    color: red;
    z-index: 10;
  }

  .todo-description {
    font-size: 12px;
    word-break: break-word;
  }
}
</style>
