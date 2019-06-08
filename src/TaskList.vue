<template>
  <ul v-else class="list-group mb-3 tasks-list">
    <!-- Vue no pasa las propiedades automaticamente, hay que hacerlo explicitatmente usando v-bind. por ejemplo: ':task="task" e :index="index"' -->
    <!-- Para respetar la correcta formacion del HTML en lugar de usar directamente la etiqueta del template <app-task> lo que debo hacer es decir que el elemento 'li' es equivalente a 'app-task' usando el atributo 'is' -->
    <li
      is="app-task"
      v-for="(task,index) in tasks"
      :tasks="tasks"
      :task="task"
      :index="index"
      @remove="deleteTask"
    ></li>
  </ul>
</template>
<script>
import Task from "./Task.vue";

export default {
  components: {
    "app-task": Task
  },
  props: ["tasks"],
  methods: {
    deleteTask: function(index) {
      this.tasks.splice(index, 1);
    }
  }
};
</script>