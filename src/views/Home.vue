<template>
  <div v-if="showAddTaskForm">
    <AddTask class="mb-3" @add-task="addTask"/>
  </div>
  <Tasks :tasks="tasks" @archive-task="archiveTask" @toggle-reminder="toggleReminder" />
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
  name: 'Home',
  components: {
    Tasks,
    AddTask
  },
  props: {
    showAddTaskForm: Boolean
  },
  data() {
    return {
      // Array vacio de tareas
      tasks: new Array()
    }
  },
  methods: {
    async addTask(newTask) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newTask)
      });

      if (!res.status.toString().startsWith('2')) {
        alert("No se pudo realizar la operación.")
        return;
      }

      const data = await res.json();

      this.tasks = [data, ...this.tasks];
    },

    async archiveTask(id) {
      if (confirm("Estás seguro de querer borrar esta tarea?")) {
        const res = await fetch(`api/tasks/${id}`, { method: 'DELETE' });
        
        if (!res.status.toString().startsWith('2')) {
          alert("Ocurrio un error al intentar eliminar la tarea.");
        }

        this.tasks = this.tasks.filter(tarea => tarea.id != id);
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = { ...taskToToggle, recordatorio: !taskToToggle.recordatorio }

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updateTask)

      });

      if (!res.status.toString().startsWith('2')) {
        alert("No se pudo realizar la operación.")
        return;
      }

      const data = await res.json();

      this.tasks = this.tasks.map(tarea =>{
        if (tarea.id === id) {
          tarea.recordatorio = data.recordatorio;
          return tarea;
        } else {
          return tarea;
        }
      });
    },

    async fetchTasks() {
      const res = await fetch("api/tasks");

      if (res.status.toString().startsWith('4') || res.status.toString().startsWith('5')) {
        alert("No se pudo obtener las tareas.")
        return [];
      }

      return res.json();
    },

    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      return res.json();
    }
  },

  async created () {
    this.tasks = await this.fetchTasks();
    this.tasks.reverse();
  },
}
</script>

<style>

</style>