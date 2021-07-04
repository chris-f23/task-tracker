<template>
  <div :class="['card mb-1', task.recordatorio ? 'reminder' : '' ]">
    <div class="card-body row" @dblclick="$emit('toggle-reminder', task.id)">
      <div class="col-10">
        <h5 class="card-title mb-3">{{ task.texto }}</h5>
        <h6 class="card-subtitle mb-2 text-muted">{{ task.fecha }}</h6>
      </div>
      <div class="col">
        <button @click="$emit('archive-task', task.id)" type="button" class="btn-close float-end" aria-label="Close"></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object
  },
  emits: ['archive-task', 'toggle-reminder']
}
</script>
<style scoped>
.reminder {
  background:navajowhite;
}
</style>