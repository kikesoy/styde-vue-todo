<template>
  <div id="app" class="container">
    <div class="col-sm-12">
      <h1>To do list</h1>
    </div>
    <div class="col-sm-12">
      <div v-if="!tasks.length" class="alert alert-info">
        <h2>
          <app-icon img="exclamation-circle"></app-icon>Bien!
          <small>No hay tareas en la lista</small>
        </h2>
      </div>
      <app-task-list :tasks="tasks"></app-task-list>
      <p>
        <a @click.prevent="deleteCompleted" href="#" class="btn btn-danger">
          <app-icon img="trash"></app-icon>Eliminar las tareas completadas
        </a>
      </p>
      <app-task-form @created="createTask"></app-task-form>
    </div>
  </div>
</template>

<script>
import TaskList from "./TaskList.vue";
import TaskForm from "./TaskForm.vue";
import Icon from "./Icon.vue";
export default {
  components: {
    "app-task-list": TaskList,
    "app-task-form": TaskForm,
    "app-icon": Icon
  },
  methods: {
    createTask(task) {
      this.tasks.push(task);
    },
    deleteCompleted: function() {
      this.tasks = this.tasks.filter(function(task) {
        return task.pending;
      });
    }
  },
  data: function() {
    return {
      new_task: "",
      tasks: [
        {
          description: "Aprender Vue.js",
          pending: true
        },
        {
          description: "Suscribirse a un curso",
          pending: true
        },
        {
          description: "Ser una paloma en vue",
          pending: true
        },
        {
          description: "Hacer la lección de Vue",
          pending: false
        }
      ]
    };
  }
};
</script>

<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
