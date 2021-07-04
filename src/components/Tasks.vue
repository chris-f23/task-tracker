<template>
  <div class="col" :key="task.id" v-for="task in tasks">
    <Task :task="task" @archive-task="$emit('archive-task', task.id)" @toggle-reminder="$emit('toggle-reminder', task.id)"/>
  </div>
  
  <!-- <h3>Tareas completadas</h3>
  <div class="col" :key="task.id" v-for="task in getFinishedTasks()">
    <Task :task="task" />
  </div> -->
</template>

<script>
import Task from "./Task.vue"

export default {
  name: 'Tasks',
  props: {
    tasks: Array
  },
  components: {
    Task
  },
  emits: ['archive-task', 'toggle-reminder']
  // methods: {
  //   getUnfinishedTasks() {
  //     return this.tasks.filter(tarea => { tarea.completado == false })
  //   },

  //   getFinishedTasks() {
  //     return this.tasks.filter() (tarea => { tarea.completado == true })
  //   }
  // }
}
</script>