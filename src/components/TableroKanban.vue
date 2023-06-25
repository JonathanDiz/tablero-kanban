<template>
  <div id="app" class="container">
    <div class="columns">
      <div class="column">
        <div class="sortable-list">
          <h2 class="title is-4">Kanban Board</h2>
          <div
            class="card vegas-card"
            v-for="task in board"
            :key="task.id"
            draggable="true"
            @dragstart="dragStart(task)"
          >
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
          </div>
          <div class="add-task">
            <input
              type="text"
              placeholder="Add Task"
              v-model="newTaskName"
              @keyup.enter="addTask"
            />
            <button @click="addTask">Add</button>
          </div>
        </div>
      </div>
      <div class="column">
        <div class="sortable-list">
          <h2 class="title is-4">In Progress</h2>
          <div
            class="card vegas-card"
            v-for="task in inProgress"
            :key="task.id"
            draggable="true"
            @dragstart="dragStart(task)"
          >
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
          </div>
        </div>
      </div>
      <div class="column">
        <div class="sortable-list">
          <h2 class="title is-4">Done</h2>
          <div
            class="card vegas-card"
            v-for="task in done"
            :key="task.id"
            draggable="true"
            @dragstart="dragStart(task)"
          >
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
          </div>
        </div>
      </div>
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
  data() {
    return {
      tasks: [
        { id: 1, name: 'Task 1', status: 'board', editing: false },
        { id: 2, name: 'Task 2', status: 'board', editing: false },
        { id: 3, name: 'Task 3', status: 'inProgress', editing: false },
        { id: 4, name: 'Task 4', status: 'done', editing: false },
      ] as Task[],
      newTaskName: "",
    };
  },
  computed: {
    board(): Task[] {
      return this.tasks.filter(task => task.status === 'board');
    },
    inProgress(): Task[] {
      return this.tasks.filter(task => task.status === 'inProgress');
    },
    done(): Task[] {
      return this.tasks.filter(task => task.status === 'done');
    },
  },
  methods: {
    dragStart(task: Task) {
      const taskJson = JSON.stringify(task);
      const event = window.event as DragEvent;
      event.dataTransfer!.setData('task', taskJson);
    },
    editTask(task: Task) {
      task.editing = true;
    },
    saveTask(task: Task) {
      task.editing = false;
      localStorage.setItem(`task-${task.id}`, JSON.stringify(task));
    },
    addTask() {
      const newTask: Task = {
        id: this.tasks.length + 1,
        name: this.newTaskName,
        status: 'board',
        editing: false,
      };
      this.tasks.push(newTask);
      this.newTaskName = "";
    },
  },
  mounted() {
    // Cargar tareas desde la caché (localStorage) al inicio
    for (let i = 0; i < localStorage.length; i++) {
      const key = localStorage.key(i);
      if (key && key.startsWith('task-')) {
        const task = JSON.parse(localStorage.getItem(key)!) as Task;
        const taskId = parseInt(key.split('-')[1]);
        const index = this.tasks.findIndex(t => t.id === taskId);
        if (index !== -1) {
          this.tasks[index] = task;
        } else {
          this.tasks.push(task);
        }
      }
    }
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