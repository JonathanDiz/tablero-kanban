<template>
  <div>
    <button class="check-button" @click="handleCheckButtonClicked">
      <span v-if="tasks.checked">&#x2705;</span>
      <span v-else>&#x25FB;</span>
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    tasks: {
      type: Object,
      required: true
    }
  },
  methods: {
    handleCheckButtonClicked() {
      this.tasks.checked = !this.tasks.checked

      // Luego, puedes guardar o actualizar la tarea en tu almacenamiento de datos (como localStorage) si es necesario

      // Ejemplo de cómo guardar la tarea en localStorage
      localStorage.setItem(`task-${this.tasks.id}`, JSON.stringify(this.tasks))
    }
  }
})
</script>

<style scoped>
.check-button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.check-button span {
  font-size: 24px;
}
</style>
