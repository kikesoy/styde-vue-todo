<template>
  <li
    class="list-group-item d-flex justify-content-between"
    :class="{editing: editing, completed: !task.pending}"
  >
    <template v-if="!editing">
      <span class="description">
        <a @click.prevent="toggleStatus()" href="#">
          <app-icon :img="task.pending ? 'square' : 'check-square'"></app-icon>
        </a>
        {{ task.description }}
      </span>
      <span>
        <a @click.prevent="edit()" href="#">
          <app-icon img="edit"></app-icon>
        </a>
        <a @click.prevent="remove()" href="#">
          <app-icon img="trash"></app-icon>
        </a>
      </span>
    </template>

    <template v-else>
      <span>
        <a href>
          <app-icon :img="editing ? 'edit' : 'square'"></app-icon>
        </a>
        <input type="text" v-model="draft">
      </span>
      <span>
        <a @click.prevent="update()" href="#">
          <app-icon img="check"></app-icon>
        </a>
        <a @click.prevent="discard()" href="#">
          <app-icon img="times"></app-icon>
        </a>
      </span>
    </template>
  </li>
</template>
<script>
import EventBus from "./event-bus.js";
import Icon from "./Icon.vue";
export default {
  components: {
    "app-icon": Icon
  },
  data: function() {
    return {
      editing: false,
      draft: ""
    };
  },
  template: "#task-template",
  props: ["task", "index"],
  created: function() {
    EventBus.$on(
      "editing",
      function(index) {
        if (this.index != index) {
          console.log("Descartando" + this.index);
          this.discard();
        }
      }.bind(this)
    );
  },
  methods: {
    toggleStatus: function() {
      this.task.pending = !this.task.pending;
    },
    edit: function() {
      console.log("editando" + this.index);

      EventBus.$emit("editing", this.index);
      /* 
      FIX ME: Reimplement this
      this.$parent.tasks.forEach(function(task) {
        task.editing = false;
      });
      */
      this.draft = this.task.description;

      this.editing = true;
    },
    update: function() {
      this.task.description = this.draft;
      this.editing = false;
    },
    discard: function() {
      this.editing = false;
    },
    remove: function() {
      this.$emit("remove", this.index);
    }
  }
};
</script>

<style lang="scss">
.tasks-list {
  .editing {
    box-shadow: inset 0 0 7px rgba(0, 0, 0, 0.25);
    a {
      color: #007bff;
      &:hover {
        color: #0069d9;
      }
    }
    input {
      border: 0 none;
      background: transparent;
    }
  }

  .completed {
    background: #efefef;
    color: #666;
    text-decoration: line-through;
    a,
    i {
      color: inherit;
    }
  }
}
</style>