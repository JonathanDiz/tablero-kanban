<template>
  <div class="card vegas-card">
    <div class="card-content">
      <p v-if="!task.editing" @dblclick="editTask(task)">{{ task.name }}</p>
      <input
        v-else
        type="text"
        v-model="task.name"
        @blur="saveTask(task)"
        @keyup.enter="saveTask(task)"
      />
    </div>
    <div class="card-actions">
      <slot></slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Task {
  id: number;
  name: string;
  status: string;
  editing: boolean;
}

export default defineComponent({
  props: {
    task: {
      type: Object as () => Task,
      required: true,
    },
  },
  methods: {
    editTask(task: Task) {
      this.$emit("edit-task", task);
    },
    saveTask(task: Task) {
      this.$emit("save-task", task);
    },
  },
});
</script>

<style scoped>
.sortable-list {
  min-height: 100px;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #0D47A1;
}

.vegas-card {
  background-color: #FFC107;
  color: #0D47A1;
}

.vegas-card p {
  margin: 0;
  padding: 10px;
}

.vegas-card input[type="text"] {
  padding: 5px;
  border: none;
  background-color: #FFF;
  color: #0D47A1;
  margin-top: 5px;
}

.add-task {
  display: flex;
  align-items: center;
  margin-top: 10px;
}

.add-task input[type="text"] {
  flex: 1;
  padding: 5px;
  border: none;
  background-color: #FFF;
  color: #0D47A1;
  margin-right: 5px;
}

.add-task button {
  background-color: #0D47A1;
  color: #FFF;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
.add-task button:hover {
  background-color: #002171;
}
</style>
