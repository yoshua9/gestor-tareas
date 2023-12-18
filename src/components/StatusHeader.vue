<script setup lang="ts">
import { TodoStatus } from "@/types";
import { PENDING, IN_PROGRESS, COMPLETED } from "@/services/Constants";
interface Status {
  status: TodoStatus;
}

const status = defineProps<Status>();

const setStatusClass = (status: String) => {
  console.log(status);
  switch (status) {
    case PENDING:
      return "pending";
    case IN_PROGRESS:
      return "in-progress";
    case COMPLETED:
      return "completed";
    default:
      return "";
  }
};
</script>

<template>
  <div
    class="status-card flex align-center justify-between"
    :class="setStatusClass(status.status)"
  >
    <h3>{{ status.status }}</h3>

    <slot :status="status.status" />
  </div>
</template>

<style scoped lang="scss">
  .status-card {
    height: 50px;
    background: var(--vt-c-white);
    margin-bottom: 20px;
    padding: 10px 10px 14px 10px;

    &.pending {
      border-top: solid 4px var(--vt-c-status-pending);
    }

    &.in-progress {
      border-top: solid 4px var(--vt-c-status-in-progress);
    }

    &.completed {
      border-top: solid 4px var(--vt-c-status-completed);
    }
  }
</style>