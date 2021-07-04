<template>
  <form class="card" @submit="onSubmit">
    <div class="card-body">
      <h5>Nueva Tarea</h5><hr class="mt-0">
      <!-- Texto -->
      <div class="mb-3 row">
        <label class="col-sm-3 col-form-label">Tarea:</label>
        <div class="col-sm-9">
          <input type="text" class="form-control" v-model="texto">
        </div>
      </div>

      <!-- Fecha -->
      <div class="mb-3 row">
        <label class="col-sm-3 col-form-label">Fecha:</label>
        <div class="col-sm-9">
          <input type="datetime-local" class="form-control" v-model="fecha">
        </div>
      </div>

      <!-- Recordatorio -->
      <div class="mb-3 row">
        <label class="col-sm-3 form-check-label">Establecer recordatorio:</label>
        <div class="col-sm-9">
          <input class="form-check-input" type="checkbox" v-model="recordatorio">
        </div>
      </div>

      <button type="submit" class="btn btn-success float-end">Agregar Tarea</button>
    </div>
  </form>

</template>

<script>
export default {
  name: 'AddTask',
  data() {
    return {
      texto: '',
      fecha: '',
      recordatorio: false
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.texto) {
        alert("Debe escribir el texto de la tarea")
        return;
      }

      const newTask = {
        id: Math.floor(Math.random() * 10000),
        texto: this.texto,
        fecha: this.fecha,
        recordatorio: this.recordatorio
      }

      this.$emit('add-task', newTask);

      this.texto = '';
      this.fecha = '';
      this.recordatorio = false;
    }
  },
  emits: ['add-task']
}
</script>

<style>

</style>