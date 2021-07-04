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
  created () {
    this.tasks = [
      new Task(1, 'Comprar mercadería', new Date(), false),
      new Task(2, 'Sacar la basura', new Date(2021, 7, 3, 12, 30), false),
      new Task(3, 'Regar las plantas del jardín', new Date(2021, 7, 3, 16, 22), false),
      new Task(4, 'Pasear al perro', new Date(2021, 7, 1, 21, 20), true)
    ]
  },
  methods: {
    addTask(newTask) {
      this.tasks = [newTask, ...this.tasks];
      this.toggleAddTaskForm();
    },

    toggleAddTaskForm() {
      this.showAddTaskForm = !this.showAddTaskForm;
    },

    archiveTask(id) {
      if (confirm("Estás seguro de querer borrar esta tarea?")) {
        this.tasks = this.tasks.filter(tarea => tarea.id != id)
      }
    },

    toggleReminder(id) {
        this.tasks = this.tasks.map(tarea =>{
          if (tarea.id === id) {
            tarea.recordatorio = !tarea.recordatorio;
            return tarea;
            // return { ...tarea, recordatorio: !tarea.recordatorio };
          } else {
            return tarea;
          }
        });
    }
  }
}

class Task {
  constructor(id, texto, fecha, recordatorio) {
    this.id = id;
    this.texto = texto;
    this.fecha = fecha.toLocaleString();
    this.recordatorio = recordatorio;
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
