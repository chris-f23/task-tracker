<template>
  <div class="container">
    <div class="mt-4 mx-2 card shadow-sm">
      <div class="card-header">
        <Header @toggle-add-task-form="toggleAddTaskForm" :showAddTaskForm="showAddTaskForm"/>
      </div>

      <div class="card-body">
        <router-view :showAddTaskForm="showAddTaskForm"></router-view>
      </div>
    </div>
  </div>
  <Footer />
</template>

<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    Footer
  },
  data() {
    return {
      showAddTaskForm: false
    }
  },
  methods: {
    toggleAddTaskForm() {
      this.showAddTaskForm = !this.showAddTaskForm;
    },
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
