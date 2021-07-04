<template>
    <div class="row">
      <div class="col-8">
        <h3 class="app-header">Task Tracker</h3>
      </div>
      <div class="col">
        <Button class="float-end"
          :texto="showAddTaskForm ? 'Cerrar' : 'Nueva Tarea'"
          :color="showAddTaskForm ? 'danger' : 'primary'"
          @btn-click="$emit('toggle-add-task-form')"/>
      </div>
    </div>
</template>

<script>
import Button from './Button.vue'

export default {
  name: 'Header',
  props: {
    showAddTaskForm: Boolean
  },
  components: {
    Button
  }
}
</script>

<style scoped>
.app-header {
  display: inline-block;
  vertical-align:middle;
}
</style>