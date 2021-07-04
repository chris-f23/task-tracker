<template>
  <div class="container">
    <div class="mt-4 mx-2 card shadow-sm">
      <div class="card-header">
        <Header @toggle-add-task-form="toggleAddTaskForm" :showAddTaskForm="showAddTaskForm"/>
      </div>
      <div class="card-body">
        <div v-if="showAddTaskForm">
          <AddTask class="mb-3" @add-task="addTask"/>
        </div>
        <Tasks :tasks="tasks" @archive-task="archiveTask" @toggle-reminder="toggleReminder" />
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTask from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data() {
    return {
      // Array vacio de tareas
      tasks: new Array(),
      showAddTaskForm: false
    }
  },
  async created () {
    this.tasks = await this.fetchTasks();
    this.tasks.reverse();
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

      if (res.status !== 200) {
        alert("No se pudo realizar la operación.")
        return;
      }

      const data = await res.json();

      this.tasks = [data, ...this.tasks];
      this.toggleAddTaskForm();
    },

    toggleAddTaskForm() {
      this.showAddTaskForm = !this.showAddTaskForm;
    },

    async archiveTask(id) {
      if (confirm("Estás seguro de querer borrar esta tarea?")) {
        const res = await fetch(`api/tasks/${id}`, { method: 'DELETE' });
        
        if (res.status === 200) {
          this.tasks = this.tasks.filter(tarea => tarea.id != id);
        } else {
          alert("Ocurrio un error al intentar eliminar la tarea.");
        }
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

      if (res.status !== 200) {
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

      if (res.status !== 200) {
        alert("No se pudo obtener las tareas.")
        return [];
      }

      return res.json();
    },

    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      return res.json();
    }
  }
}

</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
} */
</style>
